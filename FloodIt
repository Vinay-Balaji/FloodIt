import java.util.Arrays;
import java.util.ArrayList;
import tester.*;
import java.util.Random;
import javalib.impworld.*;
import java.awt.Color;
import javalib.worldimages.*;

// represents one square
class Cell {
  int x;
  int y;
  Color color;
  boolean flooded;
  Cell left;
  Cell top;
  Cell right;
  Cell bottom;

  // TEMPLATE
  /*
   * FIELDS: 
   * ... this.x ...                     - int
   * ... this.y ...                     - int
   * ... this.color ...                 - Color
   * ... this.flooded ...               - boolean
   * ... this.left ...                  - Cell
   * ... this.top ...                   - Cell
   * ... this.right ...                 - Cell
   * ... this.bottom ...                - Cell 
   * METHODS: 
   * ... this.drawSingleCell ...        - WorldImage
   */

  // basic constructor 
  Cell(int x, int y, Color color, boolean flooded) {
    this.x = x;
    this.y = y;
    this.color = color;
    this.flooded = false;
  }

  // full constructor 
  Cell(int x, int y, Color color, boolean flooded, 
      Cell left, Cell top, Cell right, Cell bottom) {
    this.x = x;
    this.y = y;
    this.color = color;
    this.flooded = flooded;
    this.left = left;
    this.top = top;
    this.right = right;
    this.bottom = bottom;
  }

  // ArrayList of colors
  static ArrayList<Color> listOfColors = new ArrayList<Color>(Arrays.asList(
      Color.BLUE, Color.GREEN, Color.RED, Color.YELLOW, Color.ORANGE));

  // draws a single cell - one rectangleImage
  WorldImage drawSingleCell(Color color) {
    return new RectangleImage(30, 30, OutlineMode.SOLID, color);
  }
}

// FloodItWorld class that handles all game function
class FloodItWorld extends World {
  ArrayList<ArrayList<Cell>> cells;
  int area = cellArea;
  int attempts = 25;
  static int cellArea = 22;
  static int boardArea = 20;
  Random rand = new Random(); 

  // TEMPLATE
  /*
   * FIELDS:
   * ... this.cells ...                    -- ArrayList
   * ... this.area ...                     -- area
   * ... this.attempts ...                 -- int
   * ... this.cellArea ...                 -- int
   * ... this.boardArea ...                -- int
   * 
   * METHODS: 
   * ... this.makeCells(ArrayList) ...     -- WorldImage
   * ... this.makeScene() ...              -- WorldScene
   * ... this.endScene() ...               -- WorldScene
   * ... this.onKeyEvent() ...             -- WorldScene
   * ... this.checkCells() ...             -- boolean
   * ... this.howManyClicks() ...          -- int
   * ... this.onMouseClicked() ...         -- WorldImage
   * ... this.onTick() ...                 -- WorldImage
   * ... this.showCell() ...               -- Cell
   * ... this.worldEnds() ...              -- WorldEnd
   * ... this.changeColor() ...            -- WorldImage
   * ... this.changeDiagonal ..            -- WorldScene
   */

  // basic constructor
  FloodItWorld(ArrayList<ArrayList<Cell>> cells, int area) {
    this.cells = cells; 
    this.area = area;
  }

  // board creation with two while loops
  FloodItWorld() {
    this.cells = new ArrayList<ArrayList<Cell>>();
    int y = 0;
    while (y < cellArea) {
      ArrayList<Cell> testScene = new ArrayList<Cell>();
      int x = 0;
      while (x < cellArea) {
        int randomNum = rand.nextInt(3);
        Cell exCell = new Cell(x, y, Cell.listOfColors.get(randomNum), false);
        testScene.add(exCell);
        x = x + 1;
      }
      this.cells.add(testScene);
      cells.get(0).get(0).flooded = true;
      y = y + 1;
    }
    this.makeCells(this.cells);
  }

  // creates squares that make up the board
  public void makeCells(ArrayList<ArrayList<Cell>> that) {
    for (ArrayList<Cell> listOfCells: that) {
      for (Cell c: listOfCells) {
        if (c.x >= area - 1) {
          c.right = null;
        }
        else {
          c.right = listOfCells.get(listOfCells.indexOf(c) + 1);
        }
        if (c.x <= 0) {
          c.left = null;
        }
        else {
          c.left = listOfCells.get(listOfCells.indexOf(c) - 1);
        }
        if (c.y >= area - 1) {
          c.bottom = null;
        }
        else {
          c.bottom = that.get(that.indexOf(listOfCells) + 1).get(listOfCells.indexOf(c));
        }
        if (c.y <= 0) {
          c.top = null;
        }
        else {
          c.top = that.get(that.indexOf(listOfCells) - 1).get(listOfCells.indexOf(c));
        }
      }
    }
  }

  // make scene function which is what is ultimately shown on the screen
  public WorldScene makeScene() {
    WorldScene that = new WorldScene(0, 0);
    // place the cells on WorldScene
    for (ArrayList<Cell> listOfCells: cells) {
      for (Cell c: listOfCells) {
        int place_x = c.x * area + area / 2;
        int place_y = c.y * area + area / 2;
        that.placeImageXY(c.drawSingleCell(c.color), place_x, place_y);
      }
    }
    return that;
  }

  // the endScene which represents the last scene displayed
  public WorldScene endScene(String string) {
    WorldScene end = this.getEmptyScene();
    TextImage show = (new TextImage(string, area, Color.BLUE));
    int place = area * area / 2;
    end.placeImageXY(show, place, place);
    return end;
  }

  // onKeyEvent function which handles when "r" is pressed
  public void onKeyEvent(String key) {
    if (key.equals("r")) {
      new FloodItWorld();
      this.attempts = 25;
      this.cells = new FloodItWorld().cells;
      this.makeScene();
    }
    else {
      this.makeScene();
    }
  }

  // uses two for loops to check which cells are submerged
  public boolean checkCells() {
    boolean check = true;
    for (ArrayList<Cell> listOfCells : cells) {
      for (Cell c: listOfCells) {
        if (!(c.flooded)) {
          check = false;
        }
      }
    }
    return check;
  }

  // function that tracks attempts made
  public int howManyClicks() {
    if (this.attempts >= 0) {
      return this.attempts;
    }
    else {
      return 0;
    }
  }

  // onMouseClicked Function which handles all activity when a mouse is clicked
  public void onMouseClicked(Posn p) {
    if (cells.get(0).get(0).color != this.showCell(p).color) {
      this.showCell(p);
      this.attempts -= 1;
    }
    this.showCell(p);
  }

  // onTick() function which changes the color when clicked diagonally
  public void onTick() {
    this.changeDiagonal();
    this.changeColor();
  }

  // showCell gets the square that is being clicked
  public Cell showCell(Posn p) {
    Cell start = null;
    for (ArrayList<Cell> listOfCells: cells) {
      for (Cell c: listOfCells) {
        if (p.y <=  c.y * FloodItWorld.cellArea + FloodItWorld.cellArea 
            && c.y * FloodItWorld.cellArea <= p.y 
            && p.x <= c.x * FloodItWorld.cellArea + FloodItWorld.cellArea
            && c.x * FloodItWorld.cellArea  <= p.x) {
          start = c;
          cells.get(0).get(0).color = start.color;
        }
      }
    }
    return start;
  } 

  // This function represents the endGame scene displaying the outcome to the user
  public WorldEnd worldEnds() {
    if (this.checkCells() && (this.howManyClicks() > 0)) {
      return new WorldEnd(true, this.endScene("Congratulations!"));
    }
    else if (this.howManyClicks() == 0) {
      return new WorldEnd(true, this.endScene("You lost"));
    }
    return new WorldEnd(false, this.makeScene());
  }

  // Changes color of square to the cell of the top left color
  public void changeColor() {
    for (ArrayList<Cell> listOfCells : cells) {
      for (Cell c: listOfCells) { 
        Boolean isCellFlooded = c.flooded;
        Color color = cells.get(0).get(0).color;
        if (isCellFlooded) {
          c.color = color;
        }
      }
    }
  }

  // handles any action with diagonal squares that may be flooded or not
  public void changeDiagonal() {
    for (ArrayList<Cell> listOfCells : cells) {
      for (Cell c: listOfCells) { 
        Cell topCell = c.top;
        Cell bottomCell = c.bottom;
        Cell leftCell = c.left;
        Cell rightCell = c.right;
        Color currColor = c.color;

        if (topCell != null 
            && c.top.color == currColor 
            && c.top.flooded) {
          c.flooded = true;
        }
        else if (bottomCell != null 
            && c.bottom.color == currColor
            && c.bottom.flooded) {
          c.flooded = true;
        }
        else if (leftCell != null 
            && c.left.color == currColor
            && c.left.flooded) {
          c.flooded = true;
        }
        else if (rightCell != null 
            && c.right.color == currColor
            && c.right.flooded) {
          c.flooded = true;
        }
      }
    }
  }
}  


//class for examples
class ExamplesFlood {
  ExamplesFlood(){}

  //Examples of Cells
  Cell Cell1;
  Cell Cell2;
  Cell Cell3;
  Cell Cell4;
  Cell Cell5;
  Cell Cell6;
  Cell Cell7;
  Cell Cell8;
  Cell Cell9;
  Cell Cell10;
  Cell Cell11;
  Cell Cell12;
  Cell Cell13;

  // Examples of FloodItWorld
  FloodItWorld FloodItWorld1;
  FloodItWorld FloodItWorld2;
  FloodItWorld FloodItWorld3;
  FloodItWorld FloodItWorld4;

  // Initialization
  void initCell() {

    // Initialized Cells
    this.Cell1 = new Cell(0 , 0, Color.GREEN, false);
    this.Cell2 = new Cell(1, 0, Color.BLUE, false);
    this.Cell3 = new Cell(0, 1, Color.YELLOW, false);
    this.Cell4 = new Cell(1, 1, Color.RED, false);
    this.Cell5 = new Cell(0, 0, Color.BLACK, false);
    this.Cell6 = new Cell(1, 0, Color.RED, false);
    this.Cell7 = new Cell(2, 0, Color.PINK, false);
    this.Cell8 = new Cell(0, 1, Color.YELLOW, false);
    this.Cell9 = new Cell(1, 1, Color.GREEN, false);
    this.Cell10 = new Cell(2, 1, Color.BLUE, false);
    this.Cell11 = new Cell(0, 2, Color.ORANGE, false);
    this.Cell12 = new Cell(1, 2, Color.MAGENTA, false);
    this.Cell13 = new Cell(2, 2, Color.CYAN, false);

    // random world with random cells
    this.FloodItWorld1 = new FloodItWorld();

    // adds a new cell to the empty world inside FloodItWorld1
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);

    // 1x1 cell world
    this.FloodItWorld2 = new FloodItWorld(world, 1);

    // generates a new world with a 2x2 cells inside FloodItWorld2
    ArrayList<ArrayList<Cell>> world2 = new ArrayList<ArrayList<Cell>>();
    world2.add(new ArrayList<Cell>());
    world2.add(new ArrayList<Cell>());
    world2.get(0).add(Cell1);
    world2.get(0).add(Cell2);
    world2.get(1).add(Cell3);
    world2.get(1).add(Cell4);


    // 2x2 cells world
    this.FloodItWorld3 = new FloodItWorld(world2, 2);

    // generates a new world with a 3x3 cells inside FloodItWorld3
    ArrayList<ArrayList<Cell>> world3 = new ArrayList<ArrayList<Cell>>();
    world3.add(new ArrayList<Cell>());
    world3.add(new ArrayList<Cell>());
    world3.add(new ArrayList<Cell>());
    world3.get(0).add(Cell5);
    world3.get(0).add(Cell6);
    world3.get(0).add(Cell7);
    world3.get(1).add(Cell8);
    world3.get(1).add(Cell9);
    world3.get(1).add(Cell10);
    world3.get(2).add(Cell11);
    world3.get(2).add(Cell12);
    world3.get(2).add(Cell13);

    // 3x3 cells world
    this.FloodItWorld4 = new FloodItWorld(world3, 3);
  }

  // drawCell() methods tests
  void testDrawCell(Tester t) {
    initCell();

    t.checkExpect(this.Cell1.drawSingleCell(Cell1.color), 
        new RectangleImage(30, 
            30, "solid", Color.GREEN));
    t.checkExpect(this.Cell2.drawSingleCell(Cell2.color), 
        new RectangleImage(30, 30, "solid", Color.BLUE));
    t.checkExpect(this.Cell3.drawSingleCell(Cell3.color), 
        new RectangleImage(30, 30, "solid", Color.YELLOW));
    t.checkExpect(this.Cell4.drawSingleCell(Color.ORANGE), 
        new RectangleImage(30, 30, "solid", Color.ORANGE));
  }

  // test for makeCells method 
  void testMakeCells(Tester t) {

    // initialize for 
    initCell();

    // check initialized status (after initialize method is run) 
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    ArrayList<ArrayList<Cell>> world2 = new ArrayList<ArrayList<Cell>>();
    world2.add(new ArrayList<Cell>());
    world2.add(new ArrayList<Cell>());
    world2.get(0).add(Cell1);
    world2.get(0).add(Cell2);
    world2.get(1).add(Cell3);
    world2.get(1).add(Cell4);
    FloodItWorld3.makeCells(world2);
    t.checkExpect(this.Cell1.right, Cell2);
    t.checkExpect(this.Cell2.bottom, Cell4);
    t.checkExpect(this.Cell1.top, null);
    t.checkExpect(this.Cell2.left, Cell1);
  }

  //Tests for makeScene
  void testMakeScene(Tester t) {

    // 1x1
    initCell();

    // Test for 1x1
    WorldScene testScene = new WorldScene(0, 0);
    testScene.placeImageXY(this.Cell1.drawSingleCell(Color.GREEN), 
        this.Cell1.x * 30 + 15, 
        this.Cell1.y * 30 + 15);
    t.checkExpect(this.FloodItWorld1.makeScene(), testScene);

    // 2x2
    initCell();

    // Test for 2x2
    WorldScene testScene2 =  new WorldScene(0, 0);
    testScene2.placeImageXY(this.Cell1.drawSingleCell(Color.GREEN), 
        this.Cell1.x * 30 + 15, 
        this.Cell1.y * 30 + 15);
    testScene2.placeImageXY(this.Cell2.drawSingleCell(Color.BLUE), 
        this.Cell2.x * 30 + 15, 
        this.Cell2.y * 30 + 15);
    testScene2.placeImageXY(this.Cell1.drawSingleCell(Color.YELLOW), 
        this.Cell3.x * 30 + 15, 
        this.Cell3.y * 30 + 15);
    testScene2.placeImageXY(this.Cell1.drawSingleCell(Color.RED), 
        this.Cell4.x * 30 + 15, 
        this.Cell4.y * 30 + 15);
    t.checkExpect(this.FloodItWorld3.makeScene(), testScene2);

    // 3x3
    initCell();

    // Test for 3x3
    WorldScene testScene3 = new WorldScene(0, 0);
    testScene3.placeImageXY(this.Cell5.drawSingleCell(Color.BLACK), 
        (this.Cell5.x * 30) + 15, 
        (this.Cell5.y * 30) + 15);
    testScene3.placeImageXY(this.Cell6.drawSingleCell(Color.RED), 
        (this.Cell6.x * 30) + 15, 
        (this.Cell6.y * 30) + 15);
    testScene3.placeImageXY(this.Cell7.drawSingleCell(Color.PINK), 
        (this.Cell7.x * 30) + 15, 
        (this.Cell7.y * 30) + 15);
    testScene3.placeImageXY(this.Cell8.drawSingleCell(Color.YELLOW), 
        (this.Cell8.x * 30) + 15, 
        (this.Cell8.y * 30) + 15);
    testScene3.placeImageXY(this.Cell9.drawSingleCell(Color.GREEN), 
        (this.Cell9.x * 30) + 15, 
        (this.Cell9.y * 30) + 15);
    testScene3.placeImageXY(this.Cell6.drawSingleCell(Color.BLUE), 
        (this.Cell6.x * 30) + 15, 
        (this.Cell6.y * 30) + 15);
    testScene3.placeImageXY(this.Cell11.drawSingleCell(Color.ORANGE), 
        (this.Cell11.x * 30) + 15, 
        (this.Cell11.y * 30) + 15);
    testScene3.placeImageXY(this.Cell12.drawSingleCell(Color.MAGENTA), 
        (this.Cell12.x * 30) + 15, 
        (this.Cell12.y * 30) + 15);
    testScene3.placeImageXY(this.Cell13.drawSingleCell(Color.CYAN), 
        (this.Cell13.x * 30) + 15, 
        (this.Cell13.y * 30) + 15);
    t.checkExpect(this.FloodItWorld4.makeScene(), testScene3);
  }

  // endScene() method tests
  void testendScene(Tester t) {
    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    WorldScene finalScene = this.FloodItWorld2.getEmptyScene();
    finalScene.placeImageXY((new TextImage("Check", 2, Color.BLACK)), 
        2, 2);
    t.checkExpect(this.FloodItWorld2.endScene("Check"), finalScene);
  }

  // changeColor() method test
  void testchangeColor(Tester t) {
    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    FloodItWorld2.changeColor();
    t.checkExpect(Cell1.color, Color.GREEN);
    initCell();
    FloodItWorld3.changeColor();
    t.checkExpect(Cell1.color, Color.GREEN);
    initCell();
    FloodItWorld4.changeColor();
    t.checkExpect(Cell6.color, Color.RED);
  }

  // howManyClicks() method test
  void testhowManyClicks(Tester t) {
    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    t.checkExpect(FloodItWorld1.howManyClicks(), 25);
    t.checkExpect(FloodItWorld2.howManyClicks() - 10, 15);
    t.checkExpect(FloodItWorld3.howManyClicks() - 20, 5);
  }

  // onMouseClicked method tests
  void testOnMouseClicked(Tester t) {
    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    Posn that = new Posn(0 * FloodItWorld.cellArea + 1, 
        0 * FloodItWorld.cellArea + 1);
    FloodItWorld3.onMouseClicked(that);
    t.checkExpect(FloodItWorld3.showCell(that), this.Cell1);
    t.checkExpect(FloodItWorld3.attempts, 25);
    initCell();
    Posn other = new Posn(1 * FloodItWorld.cellArea + 1, 
        1 * FloodItWorld.cellArea + 1);
    FloodItWorld3.onMouseClicked(other);
    t.checkExpect(FloodItWorld3.showCell(other), this.Cell4);
    t.checkExpect(FloodItWorld3.attempts, 24);
  }

  // onTickMethod() test
  void testOnTick(Tester t) {
    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    FloodItWorld3.onTick();
    t.checkExpect(Cell1.color, Color.GREEN);
    t.checkExpect(FloodItWorld3.cells.get(1).get(0).flooded, false);
  }

  // showCell() method test
  void testshowCell(Tester t) {
    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    Posn that = new Posn(0 * FloodItWorld.cellArea + 1, 
        0 * FloodItWorld.cellArea + 1);
    Posn other = new Posn(1 * FloodItWorld.cellArea + 1, 
        1 * FloodItWorld.cellArea + 1);
    t.checkExpect(FloodItWorld2.showCell(that), this.Cell1);
    t.checkExpect(FloodItWorld3.showCell(that), this.Cell1);
    t.checkExpect(FloodItWorld4.showCell(other), this.Cell9);
  }

  // onKeyEvent method test
  void testonKeyEvent(Tester t) {
    // initialize
    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    FloodItWorld1.onKeyEvent("r");
    t.checkExpect(FloodItWorld1, FloodItWorld1);
    t.checkExpect(FloodItWorld1.cells, FloodItWorld1.cells);
    t.checkExpect(FloodItWorld1.attempts, 25);

  }

  // checkCells method test
  void testcheckCells(Tester t) {

    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    t.checkExpect(FloodItWorld2.checkCells(), false);
    t.checkExpect(FloodItWorld3.checkCells(), false);
    t.checkExpect(FloodItWorld4.checkCells(), false);
  }

  // changeDiagonal method tests
  void testchangeDiagonal(Tester t) {
    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    FloodItWorld3.changeDiagonal();
    FloodItWorld4.changeDiagonal();
    t.checkExpect(FloodItWorld3.cells.get(1).get(0).flooded, false);
    t.checkExpect(FloodItWorld4.cells.get(1).get(0).flooded, false);
  }

  // Runs the world
  void testRunTheWorld(Tester t) {
    initCell();
    ArrayList<ArrayList<Cell>> world = new ArrayList<ArrayList<Cell>>();
    world.add(new ArrayList<Cell>());
    world.get(0).add(Cell1);
    this.FloodItWorld1.bigBang(FloodItWorld.cellArea * FloodItWorld1.area, 
        FloodItWorld.cellArea * FloodItWorld1.area, 0.5);
  }
}
