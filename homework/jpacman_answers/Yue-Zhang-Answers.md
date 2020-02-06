# JPacman Answers

+ Q1: what is the role of EmptySprite?

  + A1: According to the comments in *EmptySprite.java*, EmptySprite does not contain any data. When this sprite is drawn, nothing happens. It is used in both *AnimatedSprite.java* and *ImageSprite.java*. 

    In *AnimatedSprite.java*, it is used as the end of the loop:

   ```java
  private static final Sprite END_OF_LOOP = new EmptySprite();
  /**
  * @return The frame of the current index.
  */
  private Sprite currentSprite() {
      Sprite result = END_OF_LOOP;
      if (current < animationFrames.length) {
          result = animationFrames[current];
      }
      assert result != null;
      return result;
  }
   ```

  ​		In *ImageSprite.java*, a new EmptySprite is returned if the region was not in the current sprite.
  
  ```java
  @Override
  public Sprite split(int x, int y, int width, int height) {
      if (withinImage(x, y) && withinImage(x + width - 1, y + height - 1)) {
          BufferedImage newImage = newImage(width, height);
          newImage.createGraphics().drawImage(image, 0, 0, width, height, x,
              y, x + width, y + height, null);
          return new ImageSprite(newImage);
      }
      return new EmptySprite();
  }
  ```
  
  


+ Q2: what is the role of MOVE_INTERVAL and INTERVAL_VARIATION?

  + A2: They are used in Bliky, Clyde, Inky, etc. classes which extend form class Ghost. In Ghost.java these two variables are calcualted in the function getInterval() which the delay between moves.

    The MOVE_INTERVAL is the basic movement interval which was added with a random number from zero to INTERVAL_VARIATION.

    ```java
    /**
     * The time that should be taken between moves.
     *
     * @return The suggested delay between moves in milliseconds.
     */
    public long getInterval() {
        return this.moveInterval + new Random().nextInt(this.intervalVariation);
    }
    ```

    

+ Q3: if you wanted to add a fruit, which files would you need to change?

  + A3: 
  
    We can think a fruit as a pellet with a different point value. 
  
    1. Add a ***Fruit.java*** file to create Fruit class which should be similar to the Pellet class in file ***Pellet.java***.
    2. In file ***PlayerCollisions.java***, add fruitColiding(),  functions  and playerVersusFruit() function to handle the actual case of player consuming a fruit.
    3. In file ***LevelFactory.java***, add a createFruit() function to create a new fruit, like the createPellet() function.
    
  ```java
  public Fruit createFruit() {
      return new Fruit(FRUIT_VALUE, sprites.getFruitSprite());
  }
  ```
  
  ​		4. In file ***PacManSprites.java***, add a function named getFruitSprite() to create the fruit image.
  
  ```java
  public Sprite getFruitSprite() {
      return loadSprite("/sprite/orange.png");
  }
  ```
  
  ​		5. In file ***MapParser.java***, add a new swich case 'F' in function addSquare().
  
  ```java
  case 'F':
      Square fruitSquare = boardCreator.createGround();
      grid[x][y] = fruitSquare;
      levelCreator.createFruit().occupy(fruitSquare);
      break;
  ```
  
  ​		6. In file ***/src/main/resources/board.txt***, add fruit symbol 'F' to the map.
  
    And here's the result:
  
    ![image-20200118204358601](https://tva1.sinaimg.cn/large/006tNbRwgy1gb1rtgqtnhj30a80bxjrl.jpg)
  
  