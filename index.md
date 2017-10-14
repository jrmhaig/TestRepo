---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
  <div id="board" style="width: 400px"></div>
  <input type="button" id="load" value="Load" />
  <input type="button" id="backward" value="<" />
  <input type="button" id="forward" value=">" />

  <script>
    gameMoves = "";
    function loadGame() {
      $.ajax({
        url: "games/game1.pgn",
        dataType: "text",
        success: function(data) {
          gameMoves = data;
          turn = 0;
          colourToMove = 0;
          turns = gameMoves.split("\n");
          game.reset();
          board.position(game.fen());
          console.log(gameMoves);
        }
      });
    }

    var game=new Chess();
    var turn = 0;
    var colourToMove = 0;
    $("#load").on("click", loadGame);

    var turns = gameMoves.split("\n");
    console.log(turns);

    var board = ChessBoard('board', {});
    board.start;
    $("#forward").on("click", makeMove);
    $("#backward").on("click", backMove);
    board.position(game.fen());

    function backMove() {
      game.undo();
      board.position(game.fen());
      colourToMove = 1 - colourToMove;
      turn = turn - colourToMove;
    }

    function nextMove() {
      var moves = turns[turn].split(/\s+/);
      move = moves[colourToMove + 1];
      turn = turn + colourToMove;
      colourToMove = 1 - colourToMove;
      return move;
    }

    function makeMove(){
      move = nextMove();
      game.move(move);
      console.log(move);
      board.position(game.fen());
    }
  </script>

