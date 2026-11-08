# FOOTBALL LEGENDS: DREAM XI

A responsive, single-page browser football card game with seven independent 150-card box draws.

## Included
- 16-player starter squad: 11 starting players + 5 substitutes.
- 7 packs, each with exactly 150 cards: 3 Epic (107, 106, 106), 7 Highlight, 140 Standard.
- Independent remaining-card pools per pack; pulls are removed until that pack is reset.
- 1 spin = 100 coins; 10 spins = 1,000 coins; confirmation before spending.
- Player market, collection, squad/bench swapping, local guest save.
- Firebase email/password account support and Firestore cloud save.
- Trade offers with a server-side Cloud Function to validate and execute the exchange.

## Important setup
The browser game can be opened in guest mode immediately. Firebase cloud accounts and cross-device sync require your own Firebase project configuration.

1. Create a Firebase project at https://console.firebase.google.com/.
2. Add a **Web App** and copy its Firebase web configuration into `firebase-config.js`.
3. In Firebase Authentication, enable **Email/Password**.
4. Create a Firestore database.
5. Publish `firestore.rules` in Firestore Rules.
6. Install the Firebase CLI and sign in:
   - `npm install -g firebase-tools`
   - `firebase login`
7. In the project root, run `firebase init functions` if you have not already initialized Functions. Choose JavaScript and Node 20. Keep the provided `functions/index.js` and `functions/package.json`.
8. Deploy the trade function:
   - `firebase deploy --only functions`
9. Serve the app from a local web server (not `file://`) so Firebase scripts and config load consistently. For example:
   - `python -m http.server 8000`
   - Open `http://localhost:8000`

For deployment, upload `index.html` and `firebase-config.js` to Firebase Hosting or another static host. Keep `firestore.rules` and the `functions` directory in your Firebase project.

## Trading
Players can send an offer using the recipient's Firebase UID, their own tradable card, and the exact ID of the card they want. The recipient accepts through the deployed `acceptTrade` callable function. Starter cards are not tradable. Market and pack cards are tradable. Cards currently in either player's Starting XI or substitutes cannot be traded.

## Notes / limitations
- The player portraits currently use stylized initials rather than licensed player photographs. The game is fully playable without external image dependencies; verified portrait URLs can be added to the player data later.
- Each pack has its own 150-card draw pool. The 140 standard-player selection is drawn from a larger shared roster, excluding that pack's featured players so no player identity is duplicated inside a box.
- The market and card ratings are game-balancing values, not official live game ratings.
- This is a strong playable prototype. The Firestore rules allow users to update their own save document, so a determined user could manipulate their own coin balance. Before a public competitive launch, move coin spending, spins, and purchases into trusted Cloud Functions with server-side validation.
