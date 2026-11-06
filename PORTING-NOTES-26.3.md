# Проверенные исходники для порта 26.3

## Исправление 2.0.5

В CPM 26.3 `LivingEntityRendererFabric` подменяет переданный `SubmitNodeCollector` на `CPMOrderedSubmitNodeCollector.CPMSubmitNodeCollector`. NoFigura 26.3 отправляет Figura-геометрию через `FiguraSubmitUtils.submit`, но эта функция продолжает работу только если collector является `SubmitNodeStorage`. CPM-обёртка этому условию не соответствует, поэтому Figura submission тихо отбрасывался, хотя аватар находился и режим Figura был активен. `FiguraSubmitCollectorMixin` в 2.0.5 возвращает исходный collector из поля `collector` CPM-обёртки только в режимах Auto/Figura.

На 6 ноября 2026 года доступны Fabric-сборки целевых модов для Minecraft 26.3. CPM на Modrinth публикует `26.3v0.6.27e-fabric`; пользовательский latest.log показывает CPM `0.6.27f`. Официальная страница CPM 26.3 `0.6.27f` указывает fixes для editor startup crash на NeoForge, Forge/26.4 snapshot ports и overflow-copy issue; render changes в кратком списке релиза не перечислены [7]. Fabric 26.3 использует Loom 1.17, Gradle 9.6 и Loader 0.19.5. NoFigura JAR metadata требует Java 25+.

Проверенные локальные артефакты:

- CPM `CustomPlayerModels-Fabric-26.3-0.6.27e.jar`, SHA-1 `6c14dd4e5ac0593c7bf3e405a70cfa50c4c514c7`; collector signatures просмотрены через `javap`.
- NoFigura `nofigura-0.1.6.10+26.3-fabric-mc.jar`, SHA-1 `3790911bbb6aaa5381d45831dde5e8d7e2a03ac3`; mod id `figura`, target `26.3`, Java `>=25`.
- NoFigura branch `26.3` revision `9652294b45335dc33b9b7ba786b2c11215f9d809`. Его `LivingEntityRendererMixin.submitFiguraModel` выбирает `PartFilterScheme.PIVOTS`, когда `isBodyVisible` false; в 2.0.3 bridge добавил player-specific visibility override.

## User's latest.log (2026-11-06)

Пользовательский лог сообщает Minecraft 26.3, Fabric Loader 0.20.1, Fabric API 0.165.0+26.3, CPM 0.6.27f, NoFigura 0.1.6.10+26.3 и ModelBridge 2.0.3. Figura successfully authenticated to its backend; в логе нет Mixin application errors, но обычный log level не показывает per-player avatar lookup, выбранный mode, bodyVisible before/after или возвращённый render type. 2.0.4 поэтому добавляет одноразовые `Diagnostic:` строки и сохраняет прежнее rendering behavior, чтобы не делать ещё один неподтверждённый fix.

В логе также есть `Default font failed to load` после server resource pack с отсутствующим `minecraft:textures/font/global_chat.png`; это касается font/resource reload и само по себе не объясняет Figura geometry visibility.

## References

[1]: https://fabricmc.net/2026/09/15/263.html "Fabric for Minecraft 26.3"
[2]: https://modrinth.com/plugin/custom-player-models/version/26.3v0.6.27e-fabric "CPM Fabric 26.3 release 0.6.27e"
[3]: https://modrinth.com/mod/nofigura/version/8XPQ63z4 "NoFigura 0.1.6.10 for 26.3"
[4]: https://github.com/Stranik2504/NoFigura/blob/26.3/common/src/main/java/org/figuramc/figura/mixin/render/renderers/LivingEntityRendererMixin.java "NoFigura LivingEntityRenderer rendering/filter logic"
[5]: https://github.com/tom5454/CustomPlayerModels/blob/26.3/CustomPlayerModels-26.3/src/platform-shared/java/com/tom/cpm/mixin/render/LivingRendererMixin.java "CPM getRenderType hook"
[6]: https://github.com/tom5454/CustomPlayerModels/blob/26.3/CustomPlayerModels-26.3/src/platform-shared/java/com/tom/cpm/mixin/render/AvatarRendererMixin.java "CPM avatar renderer visibility hook"
[7]: https://www.curseforge.com/minecraft/mc-mods/custom-player-models-fabric/files/9198333 "CPM 26.3 Fabric 0.6.27f release notes"
[8]: https://modrinth.com/mod/fabric-api/version/nJlcvFfj "Fabric API 0.165.2 for 26.3"
