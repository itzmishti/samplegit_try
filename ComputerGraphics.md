# CG

//BRESENHAM'S LINE DRAWING ALGO

#include<stdio.h>
#include<GL/glut.h>
#include<conio.h>
#include<math.h>
#include<iostream>
using namespace std;
#include<cmath>

void draw_line(int x1, int y1, int x2, int y2) 
{
    int dx = abs(x2 - x1);
    int dy = abs(y2 - y1);
    int sx = (x1 < x2) ? 1 : -1;
    int sy = (y1 < y2) ? 1 : -1;
    int err = dx - dy;
    while (true) 
{
        glBegin(GL_POINTS);
				glVertex2i(x1, y1);
				glEnd();
        if (x1 == x2 && y1 == y2) 
				{
            break;
        }
        int e2 = 2 * err; 
				if (e2 > -dy)
			 {
            err -= dy;
            x1 += sx;
        }
        if (e2 < dx) 
				{
            err += dx; 
						y1 += sy;
        }
    }
}
void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    glColor3f(1.0f, 1.0f, 1.0f);
    draw_line(0, 200, 200, 0);
		glFlush();
}

int main(int argc, char** argv) {
glutInit(&argc, argv);
glutCreateWindow("BRESENHAMS Line");
glClearColor(0.0, 0.0, 0.0, 0.0);
glMatrixMode(GL_PROJECTION);
gluOrtho2D(0.0, 500.0, 0.0, 500.0); 
glutDisplayFunc(display); 
glutMainLoop();
}
		    
		    
		    















```cpp
//DDA  
// http://bit.ly/410TsVw

#include<GL/glut.h>
#include<math.h>
#include<stdio.h>
#include<conio.h>

void display()
{
    glClear(GL_COLOR_BUFFER_BIT);
    glColor3f(0.0, 1.0, 1.0);
    int x0, y0, x1, y1, dx, dy, p, x, y;
    glBegin(GL_POINTS);
    x0 = 100;
    x1 = 400;
    y0 = 50;
    y1 = 100;
    dx = x1 - x0;
    dy = y1 - y0;
    x = x0;
    y = y0;
    float steps;
    if (abs(dx) >= abs(dy))
    {
        steps = dx;
    }
    else
    {
        steps = dy;
    }
    dx = dx / steps;
    dy = dy / steps;

    int i = 1;
    while (i <= steps)
    {
        glVertex2f(x, y);
        x += dx;
        y += dy;
        i++;
    }
    //getch();
    //closegraph();

    glEnd();
    glFlush();
}
void myinit()
{
    glClearColor(0.0, 0.0, 0.0, 0.0);
    glColor3f(0.0, 1.0, 0.0);
    glPointSize(10.0);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(0.0, 499.0, 0.0, 499.0);
}
int main(int argc, char** argv)
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize(500, 500);
    glutInitWindowPosition(0, 0);
    glutCreateWindow("DDA");
    glutDisplayFunc(display);
    myinit();
    glutMainLoop();
};
//BresenHams
#include <GL/glut.h>
#include <stdio.h>
int x0,x1,y0,y1;
void myInit()
{
	glClear(GL_COLOR_BUFFER_BIT);
	glClearColor(0.0, 0.0, 0.0, 1.0);
	glMatrixMode(GL_PROJECTION);
	gluOrtho2D(0, 500, 0, 500);
}
void draw_pixel(int x, int y)
{
	glBegin(GL_POINTS);
	glVertex2i(x, y);
	glEnd();
}
void draw_line(int x0, int x1, int y0, int y1) {
	int dx, dy, p, x, y;
	dx = x1 - x0;
	dy = y1 - y0;
	x = x0;
	y = y0;
	p = 2 * dy - dx;
	while (x < x1)
	{
		if (p >= 0)
		{
			draw_pixel(x, y);
			y = y + 1;
			p = p + 2 * dy - 2 * dx;
		}
		else
		{
			draw_pixel(x, y);
			p = p + 2 * dy;
		}
		x = x + 1;
	}
}
void myDisplay()
{
	draw_line(x0,x1,y0,y1);
	glFlush();
}
int main(int argc, char** argv)
{
	printf("Enter (x0, x1, y0, y1)\n");
	scanf_s("%d %d %d %d", &x0, &x1, &y0, &y1);
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowSize(500, 500);
	glutInitWindowPosition(0, 0);
	glutCreateWindow("Bresenham's Line Drawing");
	myInit();
	glutDisplayFunc(myDisplay);
	glutMainLoop();
}

//Bresenhams Cicle
#include <stdio.h>
#include <math.h>
#include <GL/glut.h>

// Center of the cicle = (320, 240)
int xc = 320, yc = 240;

// Plot eight points using circle's symmetrical property
void plot_point(int x, int y)
{
    glBegin(GL_POINTS);
    glVertex2i(xc + x, yc + y);
    glVertex2i(xc + x, yc - y);
    glVertex2i(xc + y, yc + x);
    glVertex2i(xc + y, yc - x);
    glVertex2i(xc - x, yc - y);
    glVertex2i(xc - y, yc - x);
    glVertex2i(xc - x, yc + y);
    glVertex2i(xc - y, yc + x);
    glEnd();
}

// Function to draw a circle using bresenham's
// circle drawing algorithm
void bresenham_circle(int r)
{
    int x = 0, y = r;
    float pk = 3 - 2 * r;

    /* Plot the points */
    /* Plot the first point */
    plot_point(x, y);
    int k;
    /* Find all vertices till x=y */
    while (x < y)
    {
        if (pk < 0)
        {
            pk = pk + 4 * x + 6;
        }
        else
        {
            pk = pk + 4 * (x - y) + 10;
            y = y - 1;
        }
        x = x + 1;
        plot_point(x, y);
    }
    glFlush();
}

// Function to draw two concentric circles
void concentric_circles(void)
{
    /* Clears buffers to preset values */
    glClear(GL_COLOR_BUFFER_BIT);

    int radius1 = 100, radius2 = 200;
    bresenham_circle(radius1);
    //bresenham_circle(radius2);
}

void Init()
{
    /* Set clear color to white */
    glClearColor(1.0, 1.0, 1.0, 0);
    /* Set fill color to black */
    glColor3f(0.0, 0.0, 0.0);
    /* glViewport(0 , 0 , 640 , 480); */
    /* glMatrixMode(GL_PROJECTION); */
    /* glLoadIdentity(); */
    gluOrtho2D(0, 640, 0, 480);
}

void main(int argc, char** argv)
{
    /* Initialise GLUT library */
    glutInit(&argc, argv);
    /* Set the initial display mode */
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    /* Set the initial window position and size */
    glutInitWindowPosition(0, 0);
    glutInitWindowSize(640, 480);
    /* Create the window with title "DDA_Line" */
    glutCreateWindow("bresenham_circle");
    /* Initialize drawing colors */
    Init();
    /* Call the displaying function */
    glutDisplayFunc(concentric_circles);
    /* Keep displaying untill the program is closed */
    glutMainLoop();
}

//Midpoint Circle
#include <iostream>  
#include <GL/glut.h>  
using namespace std;
int pntX1 = 250, pntY1 = 300, r = 80;
void plot(int x, int y)
{
    glBegin(GL_POINTS);
    glVertex2i(x + pntX1, y + pntY1);
    glEnd();
}
void myInit(void)
{
    glClearColor(1.0, 1.0, 0.0, 0.0);
    glColor3f(0.0f, 0.0f, 0.0f);
    glPointSize(5.0);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(0.0, 640.0, 0.0, 480.0);
}
void midPointCircleAlgo()
{
    int x = 0;
    int y = r;
    float decision = 5 / 4 - r;
    plot(x, y);
    while (y > x)
    {
        if (decision < 0)
        {
            x++;
            decision += 2 * x + 1;
        }
        else
        {
            y--;
            x++;
            decision += 2 * (x - y) + 1;
        }
        plot(x, y);
        plot(x, -y);
        plot(-x, y);
        plot(-x, -y);
        plot(y, x);
        plot(-y, x);
        plot(y, -x);
        plot(-y, -x);
    }
}
void myDisplay(void)
{
    glClear(GL_COLOR_BUFFER_BIT);
    glColor3f(0.0, 0.0, 0.0);
    glPointSize(1.0);
    midPointCircleAlgo();
    glFlush();
}
void main(int argc, char** argv)
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize(640, 480);
    glutInitWindowPosition(100, 150);
    glutCreateWindow("Line Drawing Alogrithms");
    glutDisplayFunc(myDisplay);
    myInit();
    glutMainLoop();
}

//Elllipse
#include<GL/glut.h>
#include<iostream>
using namespace std;
float a, b, h, k;
void midptellipse() {
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(1, 0, 0);
	float fx, fy, d1, d2, x, y;
	x = 0;
	y = b;
	d1 = (b * b) - (a * a * b) + (0.25 * a * a);
	fx = 2 * b * b * x;
	fy = 2 * a * a * y;
	while (fx < fy) {
		glBegin(GL_POINTS);
		glVertex2i(x + h, y + k);
		glVertex2i(-x + h, y + k);
		glVertex2i(x + h, -y + k);
		glVertex2i(-x + h, -y + k);
		glEnd();
		if (d1 < 0) {
			x++;
			fx = fx + (2 * b * b);
			d1 = d1 + fx + (b * b);
		}
		else {
			x++;
			y--;
			fx = fx + (2 * b * b);
			fy = fy - (2 * a * a);
			d1 = d1 + fx - fy + (b * b);

		}
	}
	d2 = ((b * b) * ((x + 0.5) * (x + 0.5))) + ((a * a) * ((y - 1) * (y - 1))) - (a * a * b * b);
	while (y >= 0) {
		glBegin(GL_POINTS);
		glVertex2i(x + h, y + k);
		glVertex2i(-x + h, y + k);
		glVertex2i(x + h, -y + k);
		glVertex2i(-x + h, -y + k);
		glEnd();
		if (d2 > 0) {
			y--;
			fy = fy - (2 * a * a);
			d2 = d2 + (a * a) - fy;
		}
		else {
			y--;
			x++;
			fx = fx + (2 * b * b);
			fy = fy - (2 * a * a);
			d2 = d2 + fx - fy + (a * a);
		}
	}
	glFlush();
}
void myinit() {
	glClearColor(1, 1, 1, 0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(-250, 250, -250, 250);

}
int main(int argc, char** argv)
{
	std::cout << "Enter x radius for ellipse : ";
	std::cin >> a;
	std::cout << "Enter y radius for ellipse";
	std::cin >> b;
	std::cout << "\nEnter x co - ordinate of ellipse center";
	std::cin >> h;
	std::cout << "\nEnter y co - ordinate of ellipse center : &quot";
	std::cin >> k;
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowSize(500, 500);
	glutInitWindowPosition(50, 50);
	glutCreateWindow("Mid Point Ellipse");
	glutDisplayFunc(midptellipse);
	myinit();
	glutMainLoop();
	return 0;
}

//DDA with float
#include<GL/glut.h>
#include<math.h>
#include<stdio.h>
#include<conio.h>
#include<iostream>
using namespace std;
void display()
{
    glClear(GL_COLOR_BUFFER_BIT);
    glColor3f(0.0, 1.0, 1.0);
    float x0, y0, x1, y1,  x, y;
    float dx, dy;
    glBegin(GL_POINTS);
    x0 = 100;
    x1 = 400;
    y0 = 50;
    y1 = 100;
    dx = x1 - x0;
    dy = y1 - y0;
    x = x0;
    y = y0;
    float steps;
    if (abs(dx) >= abs(dy))
    {
        steps = dx;
    }
    else
    {
        steps = dy;
    }
    dx = dx / steps;
    dy = dy / steps;
    cout << "dx and dy :" << dx<< "  " << dy << endl;
    int i = 1;
    while (i <= steps)
    {
        glVertex2f(x, y);
        x=x+ dx;
        y =y+ dy;
        cout << "dy in loop :" << dy << endl;
        i++;
        std::cout << x<<" ";
        std::cout << y << " ";
    }
    //getch();
    //closegraph();

    glEnd();
    glFlush();
}
void myinit()
{
    glClearColor(0.0, 0.0, 0.0, 0.0);
    glColor3f(0.0, 1.0, 0.0);
    glPointSize(10.0);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(0.0, 499.0, 0.0, 499.0);
}
int main(int argc, char** argv)
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize(500, 500);
    glutInitWindowPosition(0, 0);
    glutCreateWindow("DDA");
    glutDisplayFunc(display);
    myinit();
    glutMainLoop();
};
```

```cpp
//ELLIPSE WORKING CODE

#include<stdio.h>
#include<GL/glut.h>
#include<conio.h>
#include<math.h>
#include<iostream>
using namespace std;

int h, k, a, b;
void plot(int x,int y)
{
	glColor3f(0.3, 0.3, 0.7);
	glPointSize(10);
	glBegin(GL_POINTS);
	glVertex2i(h + x, k + y);
	glVertex2i(h + x, k - y);
	glVertex2i(h -x, k + y);
	glVertex2i(h - x, k - y);
	glEnd();
	glFlush();
}
void display()
{
	glClear(GL_COLOR_BUFFER_BIT);

	int x = 0;
	int y = b;
	plot(x, y);
	int fx = 2*b * b * x;
	int fy = 2*a * a * y;

	int d1 = b * b + (a * a) / 4 - a * a * b;
	while (fx < fy)
	{
		plot(x, y);
		if (d1 < 0)
		{
			x = x + 1;
			fx = fx + 2 * b * b;
			d1 = d1 + fx + b * b;

		}
		else
		{
			x = x + 1;
			y = y - 1;
			fx = fx + 2 * b * b;
			fy = fy - 2 * a * a;
			d1 = d1 + fx - fy + b * b;
		}
	}
	int d2 = a * a * (y - 1) * (y - 1) + b * b * (x - 0.5) * (x - 0.5) - a * a * b * b;
	while (y >=0)
	{
		plot(x, y);
		if (d2 > 0)
		{
			y = y - 1;
			fy = fy - 2 * b * b;
			d2 = d2 - fy + a * a;
		}
		else
		{
			y = y - 1;
			x = x + 1;
			fx = fx + 2 * b * b;
			fy = fy - 2 * a * a;
			d2 = d2 + fx - fy + a * a;
		}

	}

}
void init()
{
	glClearColor(0, 0, 0, 0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(0, 500, 0, 500);

}

int main(int argc, char** argv)
{
	std::cout << "Enter the values for a , b ,h ,k :" << endl;
	std::cin >> a >> b >> h >> k;
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowPosition(100, 100);
	glutInitWindowSize(500, 500);
	glutCreateWindow("Hey ");
	init();
	glutDisplayFunc(display);
	glutMainLoop();
	return 0;
}
```

```cpp
//Midpoint Circle drawing Working Code

#include<stdio.h>
#include<GL/glut.h>
#include<conio.h>
#include<math.h>
#include<iostream>
using namespace std;

int xc, yc, r;
void plot(int x,int y)
{
	glColor3f(0.3, 0.3, 0.7);
	glPointSize(10);
	glBegin(GL_POINTS);
	glVertex2i(xc + x, yc + y);
	glVertex2i(xc + x, yc - y);
	glVertex2i(xc - x, yc + y);
	glVertex2i(xc - x, yc - y);
	glVertex2i(xc + y, yc + x);
	glVertex2i(xc + y, yc - x);
	glVertex2i(xc - y, yc + x);
	glVertex2i(xc - y, yc - x);

	glEnd();
	glFlush();
}
void display()
{
	glClear(GL_COLOR_BUFFER_BIT);
	int x = 0;
	int y = r;
	plot(x, y);
	int p = 5/4 -r;
	while (x < y)
	{
		plot(x, y);

		if (p < 0)
		{
			x = x + 1;
			p = p + 2 * x + 1;
		}
		else
		{
			x = x + 1;
			y = y - 1;
			p = p + 2*(x - y) + 1;
		}
	}

}
void init()
{
	glClearColor(0, 0, 0, 0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(0, 500, 0, 500);

}

int main(int argc, char** argv)
{
	std::cout << "Enter the values for x,y,r :" << endl;
	std::cin >> xc >> yc >> r;
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowPosition(100, 100);
	glutInitWindowSize(500, 500);
	glutCreateWindow("Hey ");
	init();
	glutDisplayFunc(display);
	glutMainLoop();
	return 0;
}
```

```cpp
//BRESENHAM'S CIRCLE DRAWING ALGO WORKING

#include<stdio.h>
#include<GL/glut.h>
#include<conio.h>
#include<math.h>
#include<iostream>
using namespace std;

//Midpoint Circle Drawing Algo 

int xc, yc, r;
void plot(int x,int y)
{
	glColor3f(0.3, 0.3, 0.7);
	glPointSize(10);
	glBegin(GL_POINTS);
	glVertex2i(xc + x, yc + y);
	glVertex2i(xc + x, yc - y);
	glVertex2i(xc - x, yc + y);
	glVertex2i(xc - x, yc - y);
	glVertex2i(xc + y, yc + x);
	glVertex2i(xc + y, yc - x);
	glVertex2i(xc - y, yc + x);
	glVertex2i(xc - y, yc - x);

	glEnd();
	glFlush();
}
void display()
{
	glClear(GL_COLOR_BUFFER_BIT);
	int x = 0;
	int y = r;
	plot(x, y);
	int p = 3-2*r;
	while (x < y)
	{
		plot(x, y);

		if (p < 0)
		{
			x = x + 1;
			p = p + 4 * x + 6;
		}
		else
		{
			x = x + 1;
			y = y - 1;
			p = p + 4*(x - y) + 10;
		}
	}

}
void init()
{
	glClearColor(0, 0, 0, 0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(0, 500, 0, 500);

}

int main(int argc, char** argv)
{
	std::cout << "Enter the values for x,y,r :" << endl;
	std::cin >> xc >> yc >> r;
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowPosition(100, 100);
	glutInitWindowSize(500, 500);
	glutCreateWindow("Hey ");
	init();
	glutDisplayFunc(display);
	glutMainLoop();
	return 0;
}
```

```cpp
//DDA ALGORITHM WORKING CODE

#include<stdio.h>
#include<GL/glut.h>
#include<conio.h>
#include<math.h>
#include<iostream>
using namespace std;

//	DDA Algo

void plot(int x,int y)
{
	glColor3f(0.3, 0.3, 0.7);
	glPointSize(10);
	glBegin(GL_POINTS);
	glVertex2i(x, y);
	glEnd();
	glFlush();
}
void display()
{
	glClear(GL_COLOR_BUFFER_BIT);
	float dx, dy;
	int steps;
	int x0, x1, y0, y1;
	x0 = 50;
	x1 = 300;
	y0 = 200;
	y1 = 50;
	dx = x1 - x0;
	dy = y1 - y0;
	float x=x0, y=y0;
	while (x < x1)
	{
		if (abs(dx) > abs(dy))
		{
			steps = dx;
		}
		else
		{
			steps = dy;
		}
		dx = dx / steps;
		dy = dy / steps;
		while (steps--)
		{
			x = x + dx;
			y = y + dy;
			plot(x, y);
		}
	}
}
void init()
{
	glClearColor(0, 0, 0, 0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(0, 500, 0, 500);

}

int main(int argc, char** argv)
{
	
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowPosition(100, 100);
	glutInitWindowSize(500, 500);
	glutCreateWindow("Hey ");
	init();
	glutDisplayFunc(display);
	glutMainLoop();
	return 0;
}

```

```cpp

//BRESENHAM'S LINE DRAWING ALGO

#include<stdio.h>
#include<GL/glut.h>
#include<conio.h>
#include<math.h>
#include<iostream>
using namespace std;
#include<cmath>

void draw_line(int x1, int y1, int x2, int y2) 
{
    int dx = abs(x2 - x1);
    int dy = abs(y2 - y1);
    int sx = (x1 < x2) ? 1 : -1;
    int sy = (y1 < y2) ? 1 : -1;
    int err = dx - dy;
    while (true) 
{
        glBegin(GL_POINTS);
				glVertex2i(x1, y1);
				glEnd();
        if (x1 == x2 && y1 == y2) 
				{
            break;
        }
        int e2 = 2 * err; 
				if (e2 > -dy)
			 {
            err -= dy;
            x1 += sx;
        }
        if (e2 < dx) 
				{
            err += dx; 
						y1 += sy;
        }
    }
}
void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    glColor3f(1.0f, 1.0f, 1.0f);
    draw_line(0, 200, 200, 0);
		glFlush();
}

int main(int argc, char** argv) {
glutInit(&argc, argv);
glutCreateWindow("BRESENHAMS Line");
glClearColor(0.0, 0.0, 0.0, 0.0);
glMatrixMode(GL_PROJECTION);
gluOrtho2D(0.0, 500.0, 0.0, 500.0); 
glutDisplayFunc(display); 
glutMainLoop();
}
```
