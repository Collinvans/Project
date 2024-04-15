# Project
Code for start screen: 
void startScreen() {
  background(0);
  fill(255);
  textSize(40);
  textAlign(CENTER, CENTER);
  text("Press Enter to Start", width/2, height/2);
}
void keyPressed() {
  if (keyCode == ENTER) {
    gameStarted = true;
    reset();
    loop();
  }
}

