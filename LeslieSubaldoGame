import java.awt.event.KeyEvent;
import java.util.*; 

public class LeslieSubaldoGame
{
  
  // set to false to use your code
  private static final boolean DEMO = false;           
  public MichaelRapaportGame dg;
  
  // Game window should be wider than tall:   H_DIM < W_DIM   
  // (more effectively using space)
  private static final int H_DIM = 5;   // # of cells vertically by default: height of game
  private static final int W_DIM = 10;  // # of cells horizontally by default: width of game
  private static final int U_ROW = 0;
  
  private int h_dim;    //new variable for height of game, when parameter constructor is used
  private int w_dim;    //new variable for width of game, when parameter constructor is used
  private int u_row;    //new variable for where user will be placed, when parameter constructor is used
  
  private Grid grid;
  private int userRow;
  private int msElapsed;
  private int timesGet;
  private int timesAvoid;
  
  private int pauseTime = 100;
  
  //More Instance Variables 
  private Random randomGen = new Random();
  private String blank = null;
  
  //Instance Variables for Image Names
  private String avoid = "avoidEvilMinion.gif";
  private String avoid2 = "avoidEvilMinion2.gif";
  private String get = "getBanana1.gif";
  private String get2 = "getBanana2.gif";
  private String user = "minionUserIMG.gif";
  
  //Instance Variables for Randomizing What Gets 
  //Put Into Each Cell
  private int blankPercent;   
  private int getPercent;
  private int avoidPercent;
  private final int U_COL = 0;
  private final int BLANK = 0;
  private final int GET = 1;
  private final int AVOID = 2;
  
  //Instance Variable for Pause Method
  private boolean isPaused = false;
  
  public LeslieSubaldoGame()
  {
    h_dim = H_DIM;
    w_dim = W_DIM;
    u_row = U_ROW;
    init();
  }
  
  public LeslieSubaldoGame(int hdim, int wdim, int uRow)
  {
    h_dim = hdim;
    w_dim = wdim;
    u_row = uRow;
    init();
  }
  
  private void init() {  
    grid = new Grid(h_dim, w_dim, Color.WHITE);   
    ///////////////////////////////////////////////
    userRow = u_row;
    msElapsed = 0;
    timesGet = 0;
    timesAvoid = 0;
    updateTitle();
    grid.setImage(new Location(userRow, 0), user);   
  }
  
  public void play()
  {
    
    while (!isGameOver())
    {
      if (isPaused) {
        grid.pause(pauseTime);
        handleKeyPress();
        continue;
      }
      grid.pause(pauseTime);
      handleKeyPress();
      if (msElapsed % (3 * pauseTime) == 0)   
      {
        scrollLeft();
        populateRightEdge();
      }
      updateTitle();
      msElapsed += pauseTime;
    }
    
  }
  
  public void handleKeyPress()
  {
    int key = grid.checkLastKeyPressed();
    
    //use Java constant names for key presses
    //http://docs.oracle.com/javase/7/docs/api/constant-values.html#java.awt.event.KeyEvent.VK_DOWN
    if (key == KeyEvent.VK_Q)
      System.exit(0);
    
    else if (key == KeyEvent.VK_UP && userRow > 0) {
      Location currentLoc = new Location(userRow--, 0);
      handleCollision(currentLoc);
      grid.setImage(new Location(userRow+1, 0), blank);
      grid.setImage(new Location(userRow, 0), user);
    }
    else if (key == KeyEvent.VK_DOWN && (userRow < h_dim-1)) {
      Location currentLoc = new Location(userRow++, 0);
      handleCollision(currentLoc);
      grid.setImage(new Location(userRow-1, 0), blank);
      grid.setImage(new Location(userRow, 0), user);
    }
    // to help you with step 9  --> explore to understand how to tune your game speed
    else if (key == KeyEvent.VK_T)      
    {
      boolean interval = (msElapsed % (3 * pauseTime) == 0);
      System.out.println("pauseTime " + pauseTime + " msElapsed reset " + msElapsed 
                        + " interval " + interval);
    }
    else if (key == KeyEvent.VK_COMMA) {     //slow down
      pauseTime += 50;
      msElapsed = 0;
    }
    else if (key == KeyEvent.VK_PERIOD) {    //speed up 
      if (pauseTime > 10) {
        pauseTime -= 10;
        msElapsed = 0;
      }
    }
    else if (key == KeyEvent.VK_P) {        //pause
      isPaused = !isPaused;
    }
  }
  
  //helper method for populateRightEdge(); determines what gets put into
  //each cell
  private int randomPopulate(int b, int g, int a, int[] arr) { 
    for (int j=0; j < b; j++) {    //adds all the BLANKs first
      arr[j] = BLANK;
      }
    for (int k=0; k < g; k++) {    //next, adds all the GETs
      arr[k+b] = GET;             //would add at the correct index in the array
    }                             //so that for loop would not go out of bounds/range
    for (int l=0; l < a; l++) {    //finally, adds all the AVOIDs
      arr[l+g+b] = AVOID;      //likewise - would add at the correct index in the array
    }
    int randomNum = randomGen.nextInt(arr.length); //randomly picks from the array
    int populate = arr[randomNum];                  
    return populate;              //returns BLANK, GET, or AVOID
  }
  
  public void populateRightEdge()
  {
    int num = 0;
    int countAvoid = 0;
    while (num < h_dim) {      
      blankPercent = randomGen.nextInt(5) + 1;    
      getPercent = randomGen.nextInt(3) + 1;     //better percentage of having gets
      avoidPercent = randomGen.nextInt(1) + 1;   //lower percentage of having avoids
      int sum = blankPercent + getPercent + avoidPercent;
      int[] randomArray = new int[sum];  
      int pop = randomPopulate(blankPercent, getPercent, avoidPercent, randomArray); 
      if (pop == BLANK)                                                          
        grid.setImage(new Location(num, w_dim-1), blank);   
      else if (pop == GET) {                                
        int n = randomGen.nextInt(2);                       
        if (n == 0) 
          grid.setImage(new Location(num, w_dim-1), get);
        else if (n == 1)
          grid.setImage(new Location(num, w_dim-1), get2);
      }
      else if (pop == AVOID) {
        if (countAvoid == w_dim - h_dim) {
          grid.setImage(new Location(num, w_dim-1), blank); //avoiding a situation where it's all avoids
        }
        else {
          countAvoid++;
          int n = randomGen.nextInt(2);
          if (n == 0)
            grid.setImage(new Location(num, w_dim-1), avoid);
          else if (n == 1)
            grid.setImage(new Location(num, w_dim-1), avoid2);
        }
      }
      num++;
    }
  }
  
  
  public void scrollLeft()
  {
    Location rightOfUser = new Location(userRow, U_COL + 1); 
    handleCollision(rightOfUser);
    for (int i=0; i<h_dim; i++) {   //outer loop keeps track of rows
      for (int j=0; j< w_dim; j++) {   //inner loop deals with columns
        Location currentLoc = new Location(i, j);
        String imgName = grid.getImage(currentLoc);
        if (j - 1 >= 0) {
          grid.setImage(new Location(i, j - 1), imgName);
          grid.setImage(new Location(i, j), blank);
        }
      }
    }
    grid.setImage(new Location(userRow, U_COL), user); //needed so that user image stays
  }
  
  public void handleCollision(Location loc)
  {
    if (grid.getImage(loc) == blank) {
      grid.setImage(loc, blank);
    }
    else if (grid.getImage(loc).equals(get) || grid.getImage(loc).equals(get2)) {
      timesGet++;
      System.out.println("G");
      if (timesGet % 50 == 0) {           //game will gradually speed up
        if (pauseTime > 10) {             //every 50 bananas the minion gets
          pauseTime -= 10;
          msElapsed = 0;
        }
      }
      grid.setImage(loc, blank);
    }
    else if (grid.getImage(loc).equals(avoid) || grid.getImage(loc).equals(avoid2)) {
      timesAvoid++;
      System.out.println("A");
      grid.setImage(loc, blank);
    }
  }
  
  public int getScore()     
  {
    int score = 0;
    score = score + (timesGet * 10);     //user gains 10 points for each "get" it gets
    score = score - (timesAvoid * 10);   //user loses 10 points for each "avoid" it runs into
    return score;
  }
  
  public void updateTitle()
  {
    grid.setTitle("Game: " + getScore() + " Hits: " + timesAvoid);
    if (getScore() == 2500)
      grid.setTitle("YOU WON!");
    if (timesAvoid == 10)
      grid.setTitle("GAME OVER. Score: " + getScore());
  }
  
  public boolean isGameOver()
  {
    int gameScore = getScore();
    if (timesAvoid == 10)       //game will be over if user runs into 10 avoids
      return true;
    return false;
  }
  
  public static void test()
  {
    if (DEMO) {       
      System.out.println("Running the demo: DEMO=" + DEMO);
      //default constructor   (4 by 10)
      //MattGame game = new MattGame();
      MattGame game = new MattGame(10, 20, 0);
      game.play();
    
    } else {
      System.out.println("Running student game: DEMO=" + DEMO);
      // !DEMO   -> your code should execute those lines when you are
      // implementing your game
      
      //test 1: with parameterless constructor
      LeslieSubaldoGame game = new LeslieSubaldoGame();
      
      //test 2: with constructor specifying grid size    IT SHOULD ALSO WORK as long as height < width
      //LeslieSubaldoGame game = new LeslieSubaldoGame(10, 20, 0);
      game.play();
    }
  }
  
  public static void main(String[] args)
  {
    test();
  }
}
