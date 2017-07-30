---
layout: post
title: Семантическое версионирование
---
На проектах удобно вести [семантическое](http://semver.org/) версионирование. Версия имеет вид `X.Y.Z[-tag.abc]`, где:
- X - мажорная версия. Меняется, если нет обратной совместимости, после крупных рефакторингов, редизайнов и т.п.
- Y - минорная версия. Меняется после добавления нового функционала при сохранении обратной совместимости.
- Z - патч-версия. Меняется после баг/хотфиксов.
- tag - предрелизный тег, например alpha, beta, rc
- abc - произвольный номер 

Для автоматической генерации семантической версии можно использовать [GitVersion](https://GitVersion.readthedocs.io/en/latest/). Эта утилита командной строки, которая позволяет на основе конфигурации веток в git репозитории вычислить версию.

[Установить](https://GitVersion.readthedocs.io/en/latest/usage/command-line/) GitVersion модно по разному, например через [chocolatey](https://chocolatey.org/): <br/>
`choco install GitVersion.portable --pre`

По умолчанию GitVersion не меняет версию на каждый коммит, что не очень удобно, если используется CI. Чтобы это исправить можно поменять режим версионирования на [Continuous Deployment](https://GitVersion.readthedocs.io/en/latest/reference/continuous-deployment/)
В этом режиме версия меняется на каждый комит, для генерации релизной версии необходимо поставить тег с нужной версией.
Для включения режима необходимо добавить конфиг в корень репозитория GitVersion.yaml:
```
mode: ContinuousDeployment
ignore:
  sha: []
branches:
  master:
    regex: master
    tag: beta
  develop:
    regex: dev(elop)?(ment)?$
    tag: alpha
```
На картинке ниже показан пример генерации версий для репозитория: <br/>
![Repo]({{ site.github.url }}\assets\Gitversion\repo.png)

GitVersion помимо генерации семантической версии, генерирует много свойств, особо полезные из которых:
```
{
  "Major":1,
  "Minor":1,
  "Patch":0,
  "MajorMinorPatch":"1.1.0",
  "SemVer":"1.1.0-alpha.4",
  "AssemblySemVer":"1.1.0.0",
  "FullSemVer":"1.1.0-alpha.4",
  "InformationalVersion":"1.1.0-alpha.4+Branch.develop.Sha.769a9ccf76f551095f0a958d0a37c08ecea28c7b",
  "NuGetVersionV2":"1.1.0-alpha0004"
}
```

Свойство NuGetVersionV2 можно использовать для имени nuget пакета. Остальные свойства можно внедрить внутрь сборки:
```
[assembly: System.Reflection.AssemblyVersion("1.1.0.0")]
[assembly: System.Reflection.AssemblyFileVersion("1.1.0.01201")]
[assembly: System.Reflection.AssemblyInformationalVersion("1.1.0-alpha.4+Branch.develop.Sha.769a9ccf76f551095f0a958d0a37c08ecea28c7b")]
```
Для этого можно использовать [MSBuild Task](https://gitversion.readthedocs.io/en/latest/usage/msbuild-task/)
