# test
test
from flask import Flask, jsonify, request
import chess

app = Flask(__name__)

@app.route('/new_game', methods=['POST'])
def new_game():
    game = chess.Game()
    return jsonify({'fen': game.fen()})

@app.route('/move', methods=['POST'])
def move():
    game = chess.Game(request.json['fen'])
    move = chess.Move.from_uci(request.json['move'])
    game.move(move)
    return jsonify({'fen': game.fen()})

if __name__ == '__main__':
    app.run(debug=True)
<!DOCTYPE html>
<html>
<head>
  <script>
    function post_move(move) {
      fetch('/move', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          fen: document.getElementById("fen").innerHTML,
          move: move
        })
      }).then(response => response.json())
      .then(data => {
        document.getElementById("fen").innerHTML = data.fen;
        // update the board display
      });
    }
  </script>
</head>
<body>
  <div id="fen">start position</div>
  <button onclick="post_move('e2e4')">e2e4</button>
  <button onclick="post_move('d2d4')">d2d4</button>
  <!-- other buttons for moves -->
</body>
</html>
