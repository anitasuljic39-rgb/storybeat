# Paroliers StoryBeat — architecture

Chaque parolier = **base commune** (`commun/`) + **un module de registre**.

- `commun/anti-ia.md` — l'arme anti-IA (spécificité, retenue, liste noire). Chargé par tous.
- `commun/format-suno.md` — mécanique Suno (champs, balises, ad-libs).
- `commun/brief-livraison.md` — lecture du questionnaire + format de rendu.
- `clash-rap.md` — registre Clash / style rap.
- `emouvoir.md` — registre émotionnel (Amour, Hommage, Naissance, Mariage).

## Ajouter un nouveau parolier

1. Crée `agents/paroliers/<nom>.md`.
2. En tête : « Charge d'abord les 3 fichiers de `commun/`. »
3. N'écris QUE ce qui est propre au registre (ne répète jamais le commun).

Restant à créer : `humour.md` (registre Faire rire) et l'orchestrateur (lit le questionnaire, extrait les détails, dirige vers le bon parolier).
