# Guilt Chess

Chess with a conscience. Capture a piece and yours turns guilty: it can still move and block, but it can't capture again until one of its own bishops comes to absolve it.

Play with a friend on two phones, or pass one phone back and forth. No accounts, no install, no server to run.

**Play it:** `https://mrmcgoothe2nd.github.io/guilt-chess/`

## How to play

1. Open the game and tap **Start online game**. You play White.
2. Tap **Share invite link** and send it to your friend.
3. When your friend opens the link, they join as Black and the game starts.

Keep the page open on both phones while you play. If someone reloads or drops signal for a moment, the game reconnects and they get their seat back. When a game ends, tap **Rematch** to play again with colors swapped.

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

## How it works

The whole game is a single `index.html` file with no build step.

- **Rules engine.** Plain JavaScript that generates legal moves with guilt applied. Check, checkmate, and castling safety are all calculated as if guilty pieces don't attack.
- **Online play.** The two phones connect directly over WebRTC through [PeerJS](https://peerjs.com/). PeerJS's free public server only introduces the phones to each other. The game itself goes phone to phone.
- **Saving.** Each phone keeps its game in the browser's local storage, so a reload picks up where you left off.

## Hosting your own copy

1. Fork this repo, or create a new public repo and upload `index.html`.
2. Go to **Settings**, then **Pages**, set the branch to `main`, and save.
3. After a minute or two, your copy is live at `https://YOUR-USERNAME.github.io/REPO-NAME/`.

Any static host works, including Netlify, Cloudflare Pages, and Vercel.

## Troubleshooting

**Stuck on "Looking for your friend's game."** Make sure your friend still has the game open. Some mobile carriers and strict work or school networks block direct phone-to-phone connections, so try moving one or both phones onto Wi-Fi.

**"That game already has two players."** Someone else already joined with that link. Start a new game and send a fresh link.

**Slow to reconnect after reloading as White.** The connection service holds the old session open for up to a minute, and the game keeps retrying until it gets through.

## License

Add a license of your choice. [MIT](https://choosealicense.com/licenses/mit/) is a common pick for small projects like this.
