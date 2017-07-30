---
layout: post
title: GitVersion
---
На проектах используется семантическая версия вида X.Y.Z[-tag.abc]Кратко:
X - мажорная версия. Меняется, если нет обратной совместимости, после крупных рефакторингов, редизайнов  и т.п.
Y - минорная версия. Меняется после добавления нового функционала при сохранении обратной совместимости.
Z - патч-версия. Меняется после баг/хотфиксов.
tag - предрелизный тег, например alpha, beta, rc
abc - произвольный номер 

Для автоматической генерации версии используется gitversion.
Установить: choco install gitversion.portable --pre
По умолчанию gitversion не меняет версию на каждый коммит, что не очень удобно, если используется CI и т.п.
Поэтому изменим режим версионирования на Continuous Deployment
Для это необходимо добавить конфиг в корень репозитория GitVersion.yaml:
mode: ContinuousDeployment
branches: {}
ignore:
  sha: []
Это позволит иметь уникальную версию на каждый коммит.
Пример генерации версий:

GitVersion помимо генерации семантической версии, генерирует много полезных свойств:
{
  "Major":1,
  "Minor":1,
  "Patch":0,
  "PreReleaseTag":"alpha.4",
  "PreReleaseTagWithDash":"-alpha.4",
  "PreReleaseLabel":"alpha",
  "PreReleaseNumber":4,
  "BuildMetaData":"",
  "BuildMetaDataPadded":"",
  "FullBuildMetaData":"Branch.develop.Sha.769a9ccf76f551095f0a958d0a37c08ecea28c7b",
  "MajorMinorPatch":"1.1.0",
  "SemVer":"1.1.0-alpha.4",
  "LegacySemVer":"1.1.0-alpha4",
  "LegacySemVerPadded":"1.1.0-alpha0004",
  "AssemblySemVer":"1.1.0.0",
  "AssemblySemFileVer":"1.1.0.0",
  "FullSemVer":"1.1.0-alpha.4",
  "InformationalVersion":"1.1.0-alpha.4+Branch.develop.Sha.769a9ccf76f551095f0a958d0a37c08ecea28c7b",
  "BranchName":"develop",
  "Sha":"769a9ccf76f551095f0a958d0a37c08ecea28c7b",
  "NuGetVersionV2":"1.1.0-alpha0004",
  "NuGetVersion":"1.1.0-alpha0004",
  "NuGetPreReleaseTagV2":"alpha0004",
  "NuGetPreReleaseTag":"alpha0004",
  "CommitsSinceVersionSource":4,
  "CommitsSinceVersionSourcePadded":"0004",
  "CommitDate":"2017-06-13"
}

TODO пример работы с GitVersion через msbuild
