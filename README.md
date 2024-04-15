# Project
Code after adding start screen: 
int ballX, ballY;
float ballSpeedX = 6; // Increased base ball speed
float ballSpeedY = 6; // Increased base ball speed
int ballSize = 20;

int paddleWidth = 100;
int paddleHeight = 20;
int paddleY;

int brickRowCount = 3;
int brickColumnCount = 7;
int brickWidth = 80;
int brickHeight = 30;
int brickPadding = 10;
int[][] bricks;

int score = 0;
int level = 1;
boolean gameStarted = false;

void setup() {
  size(800, 600);
  startScreen();
}

void draw() {
  if (gameStarted) {
    background(0);
    // Draw paddle
    fill(255);
    rect(mouseX - paddleWidth/2, height - paddleHeight, paddleWidth, paddleHeight);
    
    // Draw ball
    fill(255);
    ellipse(ballX, ballY, ballSize, ballSize);
    
    // Move ball
    ballX += ballSpeedX;
    ballY += ballSpeedY;
    
    // Check for collisions with walls
    if (ballX <= 0 || ballX >= width) {
      ballSpeedX *= -1;
    }
    if (ballY <= 0) {
      ballSpeedY *= -1;
    }
    
    // Check for collision with paddle
    if (ballY + ballSize/2 >= height - paddleHeight && ballX >= mouseX - paddleWidth/2 && ballX <= mouseX + paddleWidth/2) {
      ballSpeedY *= -1;
    }
    
    // Check for collision with bricks
    for (int i = 0; i < brickRowCount; i++) {
      for (int j = 0; j < brickColumnCount; j++) {
        int brickX = j * (brickWidth + brickPadding);
        int brickY = i * (brickHeight + brickPadding);
        if (bricks[i][j] == 1) {
          if (ballX + ballSize/2 >= brickX && ballX - ballSize/2 <= brickX + brickWidth && ballY + ballSize/2 >= brickY && ballY - ballSize/2 <= brickY + brickHeight) {
            ballSpeedY *= -1;
            bricks[i][j] = 0;
            score += 10; // Increase score when a brick is hit
          }
        }
      }
    }
    
    // Draw bricks
    for (int i = 0; i < brickRowCount; i++) {
      for (int j = 0; j < brickColumnCount; j++) {
        if (bricks[i][j] == 1) {
          fill(255, 0, 0);
          rect(j * (brickWidth + brickPadding), i * (brickHeight + brickPadding), brickWidth, brickHeight);
        }
      }
    }
    
    // Display score and level
    fill(255);
    textSize(20);
    textAlign(RIGHT);
    text("Score: " + score, width - 20, 30);
    text("Level: " + level, width - 20, 60);
    
    // Check for level completion
    boolean levelComplete = true;
    for (int i = 0; i < brickRowCount; i++) {
      for (int j = 0; j < brickColumnCount; j++) {
        if (bricks[i][j] == 1) {
          levelComplete = false;
          break;
        }
      }
    }
    
    // Move to the next level if all bricks are cleared
    if (levelComplete) {
      level++;
      reset();
    }
    
    // Check for game over
    if (ballY >= height) {
      gameOver();
    }
  }
}

void startScreen() {
  background(0);
  fill(255);
  textSize(40);
  textAlign(CENTER, CENTER);
  text("Press Enter to Start", width/2, height/2);
}

void reset() {
  ballX = width/2;
  ballY = height/2;
  ballSpeedX = (level + 5) * (random(2) > 0.5 ? 1 : -1); // Increase ball speed with level
  
  paddleY = height - paddleHeight;
  
  bricks = new int[brickRowCount][brickColumnCount];
  for (int i = 0; i < brickRowCount; i++) {
    for (int j = 0; j < brickColumnCount; j++) {
      bricks[i][j] = 1;
    }
  }
}

void gameOver() {
  textSize(40);
  textAlign(CENTER, CENTER);
  fill(255);
  text("Game Over", width/2, height/2);
  noLoop();
}

void keyPressed() {
  if (keyCode == ENTER) {
    gameStarted = true;
    reset();
    loop();
  }
}


