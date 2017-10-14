---
layout: default
---
  <div id="board" style="width: 400px"></div>
  <input type="button" id="backward" value="<" />
  <input type="button" id="forward" value=">" />

  <script>
    gameMoves = "";
    function loadGame() {
      $.ajax({
        url: "CURRENT_GAME.pgn",
        dataType: "text",
        success: function(data) {
          gameMoves = data;
          turn = 0;
          colourToMove = 0;
          turns = gameMoves.split("\n");
          game.reset();
          board.position(game.fen());
        }
      });
    }

    var game=new Chess();
    var turn = 0;
    var colourToMove = 0;

    var turns = gameMoves.split("\n");

    var board = ChessBoard('board', {});
    board.start;
    $("#forward").on("click", makeMove);
    $("#backward").on("click", backMove);
    board.position(game.fen());

    function backMove() {
      if (turn > 0 || colourToMove === 1) {
        game.undo();
        board.position(game.fen());
        colourToMove = 1 - colourToMove;
        turn = turn - colourToMove;
      }
    }

    function nextMove() {
      var moves = turns[turn].split(/\s+/);
      move = moves[colourToMove + 1];
      turn = turn + colourToMove;
      colourToMove = 1 - colourToMove;
      return move;
    }

    function makeMove(){
      if (turn < turns.length - 1) {
        move = nextMove();
        if (move) {
          game.move(move);
          board.position(game.fen());
        }
      }
    }

    $(document).ready(function() {
      loadGame();      
    });
  </script>

