#include <windows.h>
    #include <GL/glut.h>

    void cylinder(float rbase,float rtop,float height);
    void blok(float tebal,int ratiol,int ratiop);
    void bilah (float r_inner,float r_outer,float tebal,int batang);
    
    int screen_width=500;//mengatur lebar screen
    int screen_height=600;//mengatur tinggi screen
    int button_up=0,button_down=0;
    int Turn=0;
    double rotation_y=0,rotation_y_plus=-15,direction;// mengatur rotasi agar searah jarum jam
    double Rhead=0,Rheadplus=0;
    double rotate_All=0,All_plus=0;
    double Angguk=0,Anggukplus=0;
    double press=0,pressplus,pressplus1=180,pressplus2=0,pressplus3=0,pressplus4=0,pressplus5=0;

    bool Toleh=true,Tolehpress=true;
    bool RightTurn=true;
    bool speed1=true,speed2=false,speed3=false,speed4=false,speed5=false;

    //GLfloat ambient_light[]={0.3,0.3,0.45,1.0};
    GLfloat ambient_light[]={0.0,0.0,0.45,1.0};//GL_LIGHT0, GL_LIGHT1, GL_LIGHT2, GL_LIGHT3
    //GLfloat  source_light[]={0.9,0.8,0.8,1.0};
    GLfloat  source_light[]={0.8,0.8,0.8,1.0};
    //GLfloat     light_pos[]={7.0,0.0,1.0,1.0};
    GLfloat     light_pos[]={5.0,0.0,6.0,1.0};

    void init(void)
    {

    glShadeModel(GL_SMOOTH);
    glViewport(0,0,screen_width,screen_height);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluPerspective(45.0f,(GLfloat)screen_width/(GLfloat)screen_height,1.0f,1000.0f);

    glEnable (GL_DEPTH_TEST);
    glPolygonMode   (GL_FRONT_AND_BACK,GL_FILL);
    glEnable (GL_LIGHTING); // pemanggilan parameter lihghting
    glLightModelfv  (GL_LIGHT_MODEL_AMBIENT,ambient_light);
    glLightfv (GL_LIGHT0,GL_DIFFUSE,source_light);
    glLightfv (GL_LIGHT0,GL_POSITION,light_pos);
    glEnable (GL_LIGHT0);
    glEnable (GL_COLOR_MATERIAL);
    glColorMaterial (GL_FRONT,GL_AMBIENT_AND_DIFFUSE);
    }

    //membuat method risize agar saat layar di maxzimize gambar mengikuti layar sehingga tidak merubah ukuran dari kipasnya
    void resize(int width,int height)
    {
    screen_width=width;
    screen_height=height;

    glClear(GL_COLOR_BUFFER_BIT|GL_DEPTH_BUFFER_BIT);
    glViewport(0,0,screen_width,screen_height);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluPerspective(45.0f,(GLfloat)screen_width/(GLfloat)screen_height,1.0f,1000.0f);

    glutPostRedisplay();
    }

    //buat method display(method penampilan gambar
    void display(void)
    {
    glClear(GL_COLOR_BUFFER_BIT|GL_DEPTH_BUFFER_BIT); //membersihkan layar latar belakang
    glMatrixMode(GL_MODELVIEW);
    glLoadIdentity();

    glTranslatef(0.0,-15,-70);
    glPushMatrix();
    glRotatef(270,1.0,0.0,0.0);
    rotate_All+=All_plus;
    glRotatef(rotate_All,0.0,0.0,1.0);
    cylinder(2.5,1.5,16); // cilinder btang bawah2
    cylinder(2.5,2.5,6); // cilinder batang bawah1
   glPushMatrix();
   glTranslatef(0.0,0.0,14);
    glRotatef(90,0.0,1.0,0.0);
    Angguk+=Anggukplus; // page up page down
    glRotatef(Angguk,0.0,0.0,1.0);
        Anggukplus=0;
     glPushMatrix();
     glRotatef(270,0.0,1.0,0.0);
     glTranslatef(0.0,0.0,1);
     cylinder(0.5,1,4);// cilinder batang atas
     glPopMatrix();
    glutSolidTorus(1.5,2,6,16);
    glTranslatef(0.0,0.0,-2);
    cylinder(1,1,4.3);//silinder penghubung batang atas dan batang bawah
    glTranslatef(0.0,0.0,2);
    glRotatef(270,0.0,1.0,0.0);
         glPushMatrix();
         glTranslatef(0.0,0.0,10);
         glRotatef(90,1.0,0.0,0.0);
     //turn left-right for fan head  10/9/2003

    // definisikan kondisi pergerakan penolehan
     if ( Toleh==true)
     {
     if(Turn >= 60)      // max degrees right
        RightTurn =false;
     if(Turn <=-60)  // max degrees left
        RightTurn =true;
     if(RightTurn == true )
     {
     Rheadplus++;
     Turn++;
     }
     else
     {
     Rheadplus--;
     Turn--;
     }
     }
     Rhead=Rhead+Rheadplus;
     glRotatef(Rhead,0.0,1.0,0.0);
     Rheadplus=0;
     // end turn left-right for fan head

     glTranslatef(0.0,0.0,-3.0);
     cylinder(4,4,6);// silinder belakang kipas
     cylinder(1,0.5,15);//silinder tonjolan di depan kipas
     glRotatef(270,1.0,0.0,0.0);
     if(Tolehpress==true)  // press down turn left-right head button
     cylinder(0.3,0.5,6);
     else // pull up turn left-right head button
     cylinder(0.3,0.5,7);
     glRotatef(90,1.0,0.0,0.0);
         glPushMatrix();
       glTranslatef(0.0,0.0,11);
       glutWireTorus(5,7,10,64);
       glutSolidTorus(0.5,12,10,64);
       rotation_y+=rotation_y_plus;
       if(rotation_y>359)rotation_y=0;
       glRotatef(rotation_y,0.0,0.0,1.0);
       bilah(3,10,3,5); // bilah(inner radius, outer radius, thickness, qty bilah)
         glPopMatrix();
       glPopMatrix();
     glPopMatrix();
     glRotatef(90,1.0,0.0,0.0);
     glTranslatef(0.0,-1.0,-4);
     blok(2,7,10);// blok bawah(papan kontrol)

     // speed selection   11/9/2003
     glTranslatef(-6,1,14);
     glRotatef(270,1.0,0.0,0.0);
     glTranslatef(2.0,0.0,0.0);
     glPushMatrix();
     glRotatef(pressplus5,1.0,0.0,0.0);
     blok(0.5,2,2); // untuk blok tombol off
     glPopMatrix();
     glTranslatef(2.0,0.0,0.0);
     glPushMatrix();
     glRotatef(pressplus1,1.0,0.0,0.0);
     blok(0.5,2,2);// untuk blok tombol f1
     glPopMatrix();
     glTranslatef(2.0,0.0,0.0);
     glPushMatrix();
     glRotatef(pressplus2,1.0,0.0,0.0);
     blok(0.5,2,2);//untuk blok tombol f2
     glPopMatrix();
     glTranslatef(2.0,0.0,0.0);
     glPushMatrix();
     glRotatef(pressplus3,1.0,0.0,0.0);
     blok(0.5,2,2);// untuk blok tombol f3
     glPopMatrix();
     glTranslatef(2.0,0.0,0.0);
     glPushMatrix();
     glRotatef(pressplus4,1.0,0.0,0.0);
     blok(0.5,2,2);//untuk blok tombolf4
     glPopMatrix();
     pressplus5=0;
     //end of speed selection
    glPopMatrix();

    glFlush();
    glutSwapBuffers();
    }


    void cylinder(float rbase,float rtop,float height)
    {
    float i;
    glPushMatrix();
    glTranslatef(0.0,0.0,-rbase/4);
    glutSolidCone(rbase,0,32,4);//membuat objek kerucut
    for(i=0;i<=height;i+=rbase/8)
    {
    glTranslatef(0.0,0.0,rbase/8);
    glutSolidTorus(rbase/4,rbase-((i*(rbase-rtop))/height),16,16); //donat
    }
    glTranslatef(0.0,0.0,rbase/4);
    glutSolidCone(rtop,0,32,4);
    glPopMatrix();
    }



    void bilah (float r_inner,float r_outer,float tebal,int batang)
    {
    float i;
    glPushMatrix();
    glTranslatef(0.0,0.0,-tebal/4);
    cylinder(r_inner,r_inner,tebal);
    glTranslatef(0.0,0.0,tebal/2);
    glRotatef(90,0.0,1.0,0.0);
    for(i=0;i<batang;i++)
        {
        glTranslatef(0.0,0.0,r_inner);
        glRotatef(315,0.0,0.0,1.0);
        blok(0.5,r_inner*4.5,(r_outer-r_inner+(r_inner/4))*2);
        glRotatef(45,0.0,0.0,1.0);
        glTranslatef(0.0,0.0,-r_inner);
        glRotatef(360/batang,1.0,0.0,0.0);
        }
    glPopMatrix();
    }


    void blok(float tebal,int ratiol,int ratiop)
    {
    float i,j;
    glPushMatrix();
        for(i=0;i<ratiop;i++)
        {
        glTranslatef(-(ratiol+1)*tebal/2,0.0,0.0);
        for(j=0;j<ratiol;j++)
            {
            glTranslatef(tebal,0.0,0.0);
            glutSolidCube(tebal); // membuat kubus
            }
        glTranslatef(-(ratiol-1)*tebal/2,0.0,tebal);
        }
    glPopMatrix();
    }

    //efek keyboard
    void keyboard_s(int key,int x,int y)
    {
        if (rotation_y_plus !=0)
        direction=(rotation_y_plus/abs(rotation_y_plus));
        else
        direction=-1;
        switch(key)
        {
        case GLUT_KEY_UP:// menaikan kipas
            rotation_y_plus++;
            break;
        case GLUT_KEY_DOWN:// menurunkan kipas
            rotation_y_plus--;
            break;
        case GLUT_KEY_END:// stop kipas
            rotation_y_plus=0;
            speed1=false;
            pressplus1=0;
            speed2=false;
            pressplus2=0;
            speed3=false;
            pressplus3=0;
            speed4=false;
            pressplus4=0;
            pressplus5=180;
            Toleh=false;
            break;
        case GLUT_KEY_F1: //speed yang pertama
            if(speed1 == false)
            {
            rotation_y_plus=15*direction;
            speed1=true;
            pressplus1=180;
            speed2=false;
            pressplus2=0;
            speed3=false;
            pressplus3=0;
            speed4=false;
            pressplus4=0;
            if(Tolehpress == true)
                Toleh=true;
            }
            else
            {
            speed1=false;
            pressplus1=0;
            rotation_y_plus=0;
            Toleh=false;
            }
        break;
    case GLUT_KEY_F2://speed ke-2
        if(speed2 == false)
        {
        rotation_y_plus=30*direction;
        speed1=false;
        pressplus1=0;
        speed2=true;
        pressplus2=180;
        speed3=false;
        pressplus3=0;
        speed4=false;
        pressplus4=0;
        if(Tolehpress == true)
        Toleh=true;
        }
        else
        {
        speed2=false;
        pressplus2=0;
        rotation_y_plus=0;
        Toleh=false;
        }
        break;
    case GLUT_KEY_F3://speed ke-3
        if(speed3 == false)
        {
        rotation_y_plus=45*direction;
        speed1=false;
        pressplus1=0;
        speed2=false;
        pressplus2=0;
        speed3=true;
        pressplus3=180;
        speed4=false;
        pressplus4=0;
        if(Tolehpress == true)
        Toleh=true;
        }
        else
        {
        speed3=false;
        pressplus3=0;
        rotation_y_plus=0;
        Toleh=false;
        }
break;
case GLUT_KEY_F4://speed ke-4
if(speed4 == false)
{
rotation_y_plus=60*direction;
speed1=false;
pressplus1=0;
speed2=false;
pressplus2=0;
speed3=false;
pressplus3=0;
speed4=true;
pressplus4=180;

if(Tolehpress == true)
Toleh=true;
}
else
{
speed4=false;
pressplus4=0;
rotation_y_plus=0;
Toleh=false;
}
break;
case GLUT_KEY_F5: //menghentikan pergerakan menoleh kiri dan kanan
if(Tolehpress == false)
{
if(speed1==true||speed2==true||speed3==true||speed4==true)
Toleh=true;
Tolehpress=true;
}
else
{
if(speed1==true||speed2==true||speed3==true||speed4==true)
Toleh=false;
Tolehpress=false;
}
break;
case GLUT_KEY_RIGHT://mengatur tolehan kipas ke kanan secara bertahap
Rheadplus++;
Turn++;
break;
case GLUT_KEY_LEFT://mengatur tolehan kipas ke kiri secara bertahap
Rheadplus--;
Turn--;
break;
case GLUT_KEY_PAGE_UP:// mengatur kipas ke posisi atas
Anggukplus--;
break;
case GLUT_KEY_PAGE_DOWN: // mengatur kipas ke posisi bawah
Anggukplus++;
break;
}
}

// interaksi melalui mouse
    void Mouse_s(int button, int state, int x, int y)
    {
        if (state==0 && button==0)
        All_plus--;
        if (state==0 && button==2)
        All_plus++;
    }
 int main(int argc,char **argv)
    {
        glutInit(&argc,argv);
        glutInitDisplayMode(GLUT_DOUBLE|GLUT_RGB|GLUT_DEPTH);
        glutInitWindowSize(screen_width,screen_height);
        glutInitWindowPosition(0,0);
        glutCreateWindow("KAMPRET 32");
        glutDisplayFunc(display);
        glutIdleFunc(display);
        glutReshapeFunc(resize);
        glutSpecialFunc(keyboard_s);
        glutMouseFunc(Mouse_s);
        init();
        glutMainLoop();
        return(0);
    }

