# Guilt Chess

Chess with a conscience. Capture a piece and yours turns guilty: it can still move and block, but it can't capture again until one of its own bishops comes to absolve it.

Play with a friend on two phones, or pass one phone back and forth. Online games are saved, so you can close the page, put your phone away, and pick the game back up whenever it's your move.

**Play it:** https://mrmcgoothe2nd.github.io/guilt-chess/

## How to play

1. Open the game and tap **Start online game**. You play White.
2. Tap **Share invite link** and send it to your friend.
3. Your friend opens the link and joins as Black.

Neither of you has to keep the page open. Close it whenever you like and open the link again later. The tab title changes to "Your move" when it's your turn. When a game ends, tap **Rematch** to play again with colors swapped.

**Opened the link on a different phone or browser?** The game won't recognize you right away and will show you as watching. Tap your color ("I'm White" or "I'm Black") to take your seat back.

Sitting together? Choose **Play on this phone** instead. That mode also has undo and an option to turn the board each move.

## Rules

Everything is regular chess, plus these rules:

- **Capturing makes a piece guilty.** It gets a red ring. A guilty piece can move to empty squares, block, and be captured, but it can't capture.
- **Guilty pieces don't give check.** A piece that can't capture can't threaten the king.
- **Bishops grant absolution.** When you move a bishop, every guilty piece of yours on the eight squares around where it lands is forgiven. The bishop has to make the move. Walking a guilty piece up to a bishop doesn't count.
- **Bishops can't forgive themselves.** A bishop that captures needs your other bishop to clear it. Lose both bishops and every capture is permanent.
- **Fine print.** En passant counts as a capture. A promoted pawn keeps its guilt, and promoting to a bishop doesn't absolve anyone that turn. Kings can never stand next to each other.

A game ends in checkmate, stalemate, resignation, threefold repetition, 50 moves without a capture or pawn move, or when only the kings are left.

## Credit

Guilt Chess is a fan-made browser version of **Remorse Chess**, a variant created by Bardelo Bardalini on TikTok. Rule details that the original videos don't pin down, such as bishops being unable to absolve themselves, are this version's own calls.

## Setting up online play

Online games are stored in a free Firebase Realtime Database. The free Spark plan needs no credit card and is far more than two players will ever use. Setup takes about 10 minutes and only needs to be done once.

**1. Create a project.** Go to [console.firebase.google.com](https://console.firebase.google.com), sign in with a Google account, and create a project. You can turn off Google Analytics.

**2. Turn on anonymous sign-in.** In the left menu, open **Build**, then **Authentication**, and click **Get started**. On the **Sign-in method** tab, choose **Anonymous**, switch it on, and save. Players never see a login; this only gives each browser an ID so the game knows whose seat is whose.

**3. Create the database.** Open **Build**, then **Realtime Database**, and click **Create database**. Pick a location near you and choose **locked mode**. Open the **Rules** tab, replace everything with the rules below, and click **Publish**.

```json
{
  "rules": {
    "games": {
      "$code": {
        ".read": "auth != null",
        ".write": "auth != null",
        ".validate": "$code.matches(/^[A-Z]{5}$/) && newData.hasChildren(['s', 'rev']) && newData.child('s').val().length < 200000"
      }
    }
  }
}
```

**4. Get your settings.** Click the gear icon, then **Project settings**. Under **Your apps**, click the web icon (`</>`), give the app any name, and register it. Firebase shows a block of code containing `firebaseConfig`. Copy the values for `apiKey`, `authDomain`, `databaseURL`, `projectId`, and `appId`. If `databaseURL` isn't in that block, copy the link shown at the top of the Realtime Database page instead.

**5. Paste them into the game.** In this repo, open `index.html`, tap the pencil icon to edit, and search for `FIREBASE_CONFIG`. Fill in the five values between the quotes, then commit. GitHub Pages updates a minute or two later.

These settings are safe to publish. Firebase web settings identify your project but don't grant access by themselves; the database rules above decide what's allowed.

## How it works

The game is a single `index.html` file with no build step.

- **Rules engine.** Plain JavaScript that generates legal moves with guilt applied. Check, checkmate, and castling safety are all calculated as if guilty pieces don't attack.
- **Online play.** Each game is one record in Firebase Realtime Database, keyed by its 5-letter code. Both phones listen to it live. Every move is saved with a version number, so if both phones try to change the game at once, only one change wins and the other is told to retry.
- **Local play.** One-phone games are saved in the browser, so a reload picks up where you left off.

## Troubleshooting

**"Online play isn't set up yet."** The Firebase settings in `index.html` are still empty. See step 5.

**"Turn on Anonymous sign-in."** Step 2 was skipped, or the setting wasn't saved.

**"The game server refused the move."** The database rules weren't published, or they were pasted incorrectly. Redo step 3.

**You're shown as watching your own game.** You're on a different browser or phone than before, or you cleared your browsing data. Tap your color to take your seat back.

**Nothing updates.** Check that you're online. The page shows "You're offline" while the connection is down and catches up when it's back.

## License

Add a license of your choice. [MIT](https://choosealicense.com/licenses/mit/) is a common pick for small projects like this.
