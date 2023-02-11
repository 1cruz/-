setBackdrop("#222222");
var player = createSprite("player.png"); // 建立玩家檔板
var ball = createSprite("ball.png"); // 建立球
var score = 0;
var lives = 3;
var speed = 5;
var level = 1;
var bricks = [];
player.y = 450;
ball.direction = 80;

initblocks();
function ballmove(){
    ball.stepForward(speed);
    if (ball.x <=0 || ball.x>=640) {
        ball.direction = ball.direction * -1;
    }
     if (ball.y <=0 ) {
        ball.direction = -ball.direction+180; 
    }
    if (ball.y >=480 ) {
        ball.x = 320;
        ball.y = 240;
        ball.direction = 180;
        speed = 5;
        lives -= 1;
    }
}
function tablet(){
    player.y = 450;
    player.x = cursor.x;
}
forever(function () {
    ballmove();
    tablet();
    print("Score: " + score, 20, 20, "white");
    print("Lives: " + lives, 20, 40, "white");
    print("Level: " + level, 20, 60, "white");
    if (lives > 0) {
        player.x = cursor.x;
        ballmove();
    } else {
        print('game over',170,200,'white',60)
        // StopIteration()
        stop()
    }
});
ball.on('touch',player,function(){
    ball.direction = -ball.direction+180 + Math.floor(Math.random()* 30 + 15);
    //ball.y -= 30;
});
ball.on('touch',bricks,function(brick){
    ball.direction = -ball.direction + 180 + Math.floor(Math.random()* 50);
    brick.destroy();
    score ++;
    if (score %30 == 0) {
        initblocks();
        level++;
    }
    if (score % 3 == 0 && speed < 15) {
        speed++;
    }
});
function initblocks(){
    var x = 32;
    var y = 16;
    for (var j=0;j<3;j++){
        for(var i=0 ;i<10; i++){
            var brick = createSprite('b0.png','b1.png','b2.png','b3.png','b4.png','b5.png')
            //var a = Math.floor(Math.random() * 5);
            brick.costumeId = Math.floor(Math.random() * 5);
            brick.x = 32+i*64;
            brick.y = 16+j*32;
            bricks.push(brick);
            //bricks[i].x = x;
            //bricks[i].y = y;
            //bricks[i].costumeId = a;
            //x += 64;
        }
        //y +=32
    }
}

