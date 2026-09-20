# Pipette (Пипетка)

A small floating color picker for macOS: grab a color anywhere on screen, see it as HEX / RGB / CMYK / HSB, and send it straight to Photoshop as the foreground color. CMYK numbers are computed by Photoshop itself (its own Color Settings), so they match. *[Русская версия ниже.](#пипетка-pipette)*

## Install

**One command, nothing else needed:**

```bash
curl -fsSL -o /tmp/Pipette.dmg https://github.com/white-currant/Pipette/releases/latest/download/Pipette.dmg \
  && hdiutil attach -nobrowse -quiet -mountpoint /tmp/Pipette-dmg /tmp/Pipette.dmg \
  && rm -rf /Applications/Pipette.app && cp -R /tmp/Pipette-dmg/Pipette.app /Applications/ ; \
hdiutil detach -quiet /tmp/Pipette-dmg; rm -f /tmp/Pipette.dmg
```

**With Homebrew:**

```bash
brew tap white-currant/tap
brew trust white-currant/tap
brew install --cask pipette
```

**Or by hand:** download `Pipette.dmg` from [Releases](https://github.com/white-currant/Pipette/releases) and drag Pipette to Applications. Updates arrive automatically via [Sparkle](https://sparkle-project.org) or from the "Check for Updates…" menu.

Permissions: **Screen Recording** (live loupe) and **Automation → Adobe Photoshop** (asked on first send).

Uninstall: `brew uninstall --zap --cask pipette`, or delete `/Applications/Pipette.app`.

---

# Пипетка (Pipette)

Маленькое плавающее окно поверх всех окон: берёт цвет с любого места экрана.

- **Взять цвет** (⌘P) — экран накрывается прозрачным слоем с крестиком, в окне живая лупа 13×13 и цвет под курсором. Клик / ⏎ — взять, Esc / правая кнопка — отмена. Клик не уходит в приложение под курсором.
- Взятый цвет: HEX, RGB, CMYK, HSB. Клик по строке копирует значение.
- **CMYK как в Photoshop.** Если Photoshop запущен, числа считает он сам (`SolidColor.cmyk` через `do javascript`) по своим Color Settings — рабочий CMYK, интент, BPC, движок ACE; подпись у строки «FOGRA39 · Photoshop». Если рабочий RGB в Photoshop не sRGB, цвет передаётся через Lab, чтобы экранный sRGB не читался как другой RGB. Без Photoshop — ColorSync по тому же профилю (Relative Colorimetric, без BPC — ближе всего к ACE, расхождение 1–3 единицы), подпись «FOGRA39 · ≈». Профиль можно выбрать вручную из установленных (Adobe Recommended, /Library/ColorSync, система); тогда считает ColorSync.
- История последних 12 цветов, сохраняется между запусками.
- **→ Photoshop** (⌘↩) — ставит основной или фоновый цвет через AppleScript (`foreground color`), любая версия по bundle id `com.adobe.Photoshop`. Галочка «сразу ставить цвет в Photoshop» делает это при каждом захвате.
- «копировать HEX при захвате» — универсальный путь в любой другой редактор (Affinity, Figma и т.п.): вставить в поле HEX.
- **Системная пипетка** (⇧⌘P) — стандартная лупа macOS (`NSColorSampler`), работает без разрешений; в неё же уходит захват, если нет права на запись экрана.

## Разрешения

- **Запись экрана** — нужна для живой лупы и своего захвата (ScreenCaptureKit). Системные настройки → Конфиденциальность и безопасность → Запись экрана и системного звука. После выдачи перезапустить приложение.
- **Автоматизация → Adobe Photoshop** — спросит при первой отправке цвета.
