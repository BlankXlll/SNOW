# SNOW
PGraphics SnowPGraphic;
PFont SnowFont;

color SnowBackgroundColor = color(0);
color SnowTypeColor = color(100);
color SnowColor = (255);

ArrayList<Drop> drops;

float[] numA = new float[800];
float[] numB = new float[400];
float[] numZ = new float[20];
//Drop[] drops = new Drop[5000];

float hx;
float hy;
float hz;
int lx;

void setup() {
  size(1920, 1080);

  SnowPGraphic = createGraphics(width, height);
  SnowFont = createFont("Helvetica-Bold", 500);
  SnowPGraphic.beginDraw();
  SnowPGraphic.background(SnowBackgroundColor);
  SnowPGraphic.noStroke();
  SnowPGraphic.fill(SnowTypeColor);
  SnowPGraphic.textAlign(CENTER, CENTER);
  SnowPGraphic.textFont(SnowFont);
  SnowPGraphic.text("ANGEL", SnowPGraphic.width/2, SnowPGraphic.height/2);
  SnowPGraphic.endDraw();

  drops = new ArrayList<Drop>();
}


void mouseReleased() {
  for (Drop mod : drops) {
    if (mod.CheckSnow()) {
      mod.stoppedOnce = true;
    }
  }
}

void draw() {
  background(0);

  //image(SnowPGraphic,0,0);

  for (lx=0; lx<10; lx++) {

    numA [lx]= random(1920);
    hx=numA[lx]; 
    drops.add(new Drop(hx));
    if (lx>3000) {
      lx=0;
    }
  }


  for (Drop mod : drops) {
    mod.fall();
    mod.show();
  }

  //saveFrame("frames/####.png");
}


class Drop {
  float x;
  float y;
  float z;
  float xspeed;
  float yspeed;
  int miniSize= 1;
  int maxSize = 8;
  int size;
  int direction;



  Boolean hitTheSnow = false;
  Boolean stoppedOnce = false;

  Drop(float ax) {
    x  = map(ax, 0, width, 0, 1)+random(0, width);
    y  = map(ax, 0, width, 0, 1)+random(0, height);
    z  = map(ax, 0, width, 0, 1)+random(0, 20);
    yspeed = map(z, 0, 20, 1, 3);
    xspeed = map(z, 0, 20, 1, 3);
    
    //x  = random(0,width);
    //y  = random(0,-100);
    ////z  = map(ax, 0, width, 0, 1)+random(0, 20);
    //yspeed = random(1, 3);
    //xspeed = random(1, 3);
    
    size = round(random(miniSize, maxSize));
    direction = round(random(0, 1));

    //delay = 200;
    //age = 0;
  }

     

  void fall() {
    //print("fall");
    //CheckSnow();
    //if (CheckSnow()) {
    //  if (mousePressed) {
    //    Velocity();
    //    hitTheSnow = false;
    //  }
    //  if (stoppedOnce) {
    //    Velocity();
    //  }
    //} else { 
    //  Velocity();
    //  if (CheckSnow()) {
    //    stoppedOnce = true;
    //  }
    //}

// if the snow is over a letter stop it
if (CheckSnow() && stoppedOnce != true){
  
  if (mousePressed) {
    Velocity();
  } 
} else {
  
    Velocity();
  
}

//if (stoppedOnce == true){
//size = 20;
//}



    if (x>width) {
      x = random(-500, -50);
      xspeed = map(z, 0, 20, 1, 3);
    }

    if (y > height) {
      y = random(-200, -100);
      yspeed = map(z, 0, 10, 1, 3);
      hitTheSnow = false;
    }
  }



  void show() {
    fill(SnowColor);
    noStroke();
    ellipse(x, y, size, size);
  }


  Boolean CheckSnow() {
    if (SnowOverLetter()) {
      hitTheSnow = true;
    }
    return hitTheSnow;
  }

  void Velocity() {
    y = y + yspeed;
    float grav = map(z, 0, 10, 0, 0.001);
    yspeed = yspeed + grav;

    if (direction==0) {
      x+= map(size, miniSize, maxSize, 0.1, 0.8);
    } else {
      x-= map(size, miniSize, maxSize, 0.1, 0.8);
    }
  }

  Boolean SnowOverLetter() {
    color checkColor = SnowPGraphic.get(int(x), int(y));
    if (checkColor == SnowTypeColor) {
      return true;
    } else {
      return false;
    }
  }
}
