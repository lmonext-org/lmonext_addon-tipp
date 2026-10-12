# Changelog: tipp-Addon (LMOnext)

Dieses Addon war bis LMOnext 1.9.0-beta Teil des Core-Pakets. Mit der
Einführung des Addon-Manager-Frameworks (Beitrag Torsten Hofmann) wurde es
als eigenständiges, self-contained Paket extrahiert.

Die vollständige Entwicklungshistorie bis zur Extraktion steht im
CHANGELOG.md des LMOnext-Kernprojekts unter den Abschnitten
`addon/tipp/*`.

## Version 1.5.0

- Sprachdateien lang/de.php + lang/en.php neu hinzugefügt: enthalten jetzt
  alle Tippspiel-Sprachschlüssel (nav_tippspiel, 150× tipp_* für den
  Admin-Bereich, 193× tf_tipp_* für das Frontend), zuvor im LMOnext-Core
  verteilt auf lang/admin/{de,en}.php und lang/frontend/{de,en}.php. Werden
  von AddonManager::loadLanguages() geladen und über
  registerAddonTranslations() automatisch in t()/tf() eingemischt
  (Core-Strings haben bei Namenskollisionen Vorrang).

## Version 1.4.1

- Als eigenständiges addon.json-Paket verpackt (Templates/Sprachdateien
  jetzt lokal im Addon statt zentral im Core), installierbar über
  Administrator → Addons.

## min_core_version-Korrektur

- min_core_version von "1.4.0" auf "1.9.0" korrigiert (Copy-Paste-Rest aus
  einer älteren internen Versionierung, faktisch wirkungslos, da niedriger
  als jede real existierende LMOnext-Version - AddonManager konnte damit nie
  eine zu alte Core-Version blockieren). "1.9.0" ist die tatsächliche
  LMOnext-Core-Version, ab der der Addon-Manager überhaupt existiert.

## Version 1.5.2

- Bugfix: "db_tables" in addon.json korrigiert - die bisherige Deklaration
  ("lmo_tipp_tipper", "lmo_tipp_tipps", "lmo_tipp_settings") stimmte weder
  in der Benennung noch vollständig mit den tatsächlich vom Addon
  angelegten Tabellen überein (echte Namen: tipp_user, tipp_team,
  tipp_liga_freigabe, tipp_abo, tipp_tipp, tipp_settings - OHNE
  DB-Präfix, der wird vom neuen "Daten löschen"-Feature im Addon-Manager
  zur Laufzeit selbst ergänzt, siehe Core-CHANGELOG.md
  src/Addon/AddonManager.php 1.4.0). Betraf bisher keine sichtbare
  Funktion, wäre aber beim neuen Purge-Feature auf einen unvollständigen/
  falschen Cleanup hinausgelaufen.

## Version 1.5.3

- Bugfix: Die Admin-Einstellung "max. Anzahl Spieltage im Voraus tippen"
  (max_spieltage_voraus) wurde zwar gespeichert, aber nirgends ausgewertet -
  es konnten immer alle Spieltage bis zum letzten getippt werden. Jetzt
  gilt: Bezugspunkt ist der aktuelle Spieltag (erster Spieltag mit noch
  tippbaren Partien); 0 = nur dieser, 1 = dieser + 1 weiterer usw.,
  "unbegrenzt" = keine Begrenzung. Spieltage außerhalb sind im Formular
  gesperrt (mit Hinweis) und werden auch serverseitig beim Speichern
  abgelehnt. Neue Funktionen tippMaxTippbarerSpieltag() und
  tippSpieltagFreigegeben() in frontend_tipp.php 0.11.0,
  view_tippspiel_frontend.php 1.20.3, neuer Sprachschlüssel
  tf_tipp_spieltag_gesperrt. Nachtragen durch den Admin bleibt unberührt.
