# TicToc
A basic Tic Tac Toe game built using HTML/JavaScript/CSS. No dependencies.


#How to get started
To make a move, the player will use the mouse single click to mark a space. In this version, there is no provision to undo the move. As soon as the move is done, the game proceeds to the next player’s chance.
At each move, The game will show whose move (Player A or Player B) it is. When the game ends, it displays one of the following three outcomes:
Winner: Player A

Winner: Player B

Draw is Blue and Black

// to start the code of the tictoc
var renderState = {
    minute: {style: 'blue', scale: 0.80, angle: 0},
    hour: {style: 'red', scale: 0.51, angle: 0}
  };

// for the drawhand and the function of the ctx 
  
  var drawHand = function(handState, ctx, cx, cy, maxRadius) {
    ctx.lineWidth = 8;
    ctx.strokeStyle = handState.style;
    ctx.beginPath();
    ctx.moveTo(cx, cy);
    ctx.lineTo(cx + Math.sin(handState.angle) * handState.scale * maxRadius,
               cy + Math.cos(handState.angle) * handState.scale * maxRadius);
    ctx.stroke();
  };

 // the 'draw' event is being emitted after each call to rocky.requestDraw() but
// at most once for each screen update, even if .requestDraw() is called frequently
rocky.on('draw', function(drawEvent) {
    var ctx = drawEvent.context;
    var w = ctx.canvas.clientWidth;
    var h = ctx.canvas.clientHeight;
  
    // clear canvas on each render
    ctx.fillStyle = 'black';
    ctx.fillRect(0, 0, w, h);
  
    // center point
    var cx = w / 2;
    var cy = h / 2;
    var maxRadius = Math.min(w, h - 2 * 10) / 2;
    drawHand(renderState.minute, ctx, cx, cy, maxRadius);
    drawHand(renderState.hour, ctx, cx, cy, maxRadius);
  
    // Draw a 12 o clock indicator
    drawHand({style: 'blue', scale: 0, angle: 0}, ctx, cx, 8, 0);
    // overdraw center so that no white part of the minute hand is visible
    drawHand({style: 'red', scale: 0, angle: 0}, ctx, cx, cy, 0);
  });
  
  // listener is called on each full minute and once immediately after registration
  rocky.on('minutechange', function(e) {
    // WatchfaceHelper will later be extracted as npm module
    var wfh = new Rocky.WatchfaceHelper(e.date);
    renderState.minute.angle = wfh.minuteAngle;
    renderState.hour.angle = wfh.hourAngle;
    rocky.requestDraw();
  });
