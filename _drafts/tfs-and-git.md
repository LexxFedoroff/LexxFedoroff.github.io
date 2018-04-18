---
layout: post
title: Выполнение git скриптов под агентом tfs
---
Основная статья https://docs.microsoft.com/en-us/vsts/build-release/actions/scripts/git-commands?view=vsts
```
  <Target Name="_clone_front">
    <PropertyGroup>
      <AuthHeader Condition="'$(System_AccessToken)' != ''">-c http.extraheader="AUTHORIZATION: bearer $(System_AccessToken)"</AuthHeader>
      <GitClone>$(GitExe) $(AuthHeader) clone $(RepoUrl) $(SrcFront)</GitClone>
      <GitCheckout>$(GitExe) checkout develop --force</GitCheckout>
      <GitPull>$(GitExe) $(AuthHeader) pull --ff-only </GitPull>
    </PropertyGroup>
    <Message Text="Pulling front..." Importance="Low"/>
    <Exec Command="$(GitClone)" Condition="!Exists($(SrcFront))" />
    <Exec Command="$(GitCheckout)" Condition="Exists($(SrcFront))" WorkingDirectory="$(SrcFront)" />
    <Exec Command="$(GitPull)" Condition="Exists($(SrcFront))" WorkingDirectory="$(SrcFront)" />
    <Message Text="Front is up to date" Importance="high" />
  </Target>
```
