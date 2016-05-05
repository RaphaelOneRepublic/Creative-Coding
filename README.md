#用Openframeworks与XCode进行创意编程

##环境搭建

直接在官网下载，将文件夹拖到随便什么地方。

例如/Users/Username/of_v0.9.3_osx_release

传送门：[Download](http://openframeworks.cc/download/)

##文档
传送门：[Learning Documents](http://openframeworks.cc/learning/)

##画图

还是传送门：[Openframeworks: Introduction To Graphics](http://openframeworks.cc/ofBook/chapters/intro_to_graphics.html)

### 创建项目
projectGenerator-osx  ——> projectGenerator 

项目名称Test
###更改Scheme
把Scheme改成当前项目的Debug 
（XCode自动判断出来的模块一般是不对的）
###函数
1. void setup(); 项目初始化
2. void draw(); 在窗口内画图

###画矩形
	if(ofGetMousePressed(OF_MOUSE_BUTTON_LEFT)) { 
	 
	// If the left mouse button is pressed...
	
	ofSetColor(255);
	ofSetRectMode(OF_RECTMODE_CENTER);
	ofDrawRectangle(ofGetMouseX(), ofGetMouseY(), 50, 50);
	
    // Draw a 50 x 50rect centered over the mouse
    
    }
为了使得矩形能够在背景上保留下来，在update函数内，加入如下代码：

	ofSetBackgroundAuto(false);
同时，为了使得背景变为黑色：加入：
	
	ofBackground(0);
右键时清除屏幕，

	if (ofGetMousePressed(OF_MOUSE_BUTTON_RIGHT)) 
	{ 
		ofBackground(0);
    }
设置随机的颜色（黑白色）

	float randomColor = ofRandom(50, 255);
	ofSetColor(randomColor);
设置随机的颜色（彩色）

	ofSetColor(ofRandom(0,255),ofRandom(0,255),ofRandom(0,255)）;
设置位置偏移
	
	float width = ofRandom(5, 20);
    float height = ofRandom(5, 20);
    float distance = ofRandom(35);
    
    float angle = ofRandom(ofDegToRad(360.0));

    float xOffset = cos(angle) * distance;
    float yOffset = sin(angle) * distance;
    ofDrawRectangle(ofGetMouseX()+xOffset, ofGetMouseY()+yOffset, width, height);
###画圆形，制造烟雾效果

	int maxRadius = 100;  // Increase for a wider brush

	int radiusStepSize = 5; 
	// Decrease for more circles (i.e. a more opaque brush)
	
	int alpha = 3;  
	// Increase for a more opaque brush
	
	int maxOffsetDistance = 100;  
	
	
	for (int radius=maxRadius; radius>0; radius-=radiusStepSize) {
    	float angle = ofRandom(ofDegToRad(360.0));
    	float distance = ofRandom(maxOffsetDistance);
    	float xOffset = cos(angle) * distance;
    	float yOffset = sin(angle) * distance;
    	ofSetColor(255, alpha);
    	ofDrawCircle(ofGetMouseX()+xOffset, ofGetMouseY()+yOffset, radius);
	}


诡异的配色线性插值

官网上相当诡异的红色

	ofColor myOrange(255, 132, 0, alpha);
	ofColor myRed(255, 6, 0, alpha);
	ofColor inBetween = myOrange.getLerped(myRed, ofRandom(1.0));
	ofSetColor(inBetween);
	
我调的，还是很诡异的蓝色。这个线性插值就是很坑
	
	ofColor myOrange(65, 132, 87, alpha);
	ofColor myRed(98, 68, 233, alpha)
	ofColor inBetween = myOrange.getLerped(myRed, ofRandom(1.0));
	ofSetColor(inBetween);
	
###用线段画星星
	int numLines = 30;
	int minRadius = 25;
	int maxRadius = 125;
	for (int i=0; i<numLines; i++)
	 {
	    float angle = ofRandom(ofDegToRad(360.0));
	    float distance = ofRandom(minRadius, maxRadius);
	    float xOffset = cos(angle) * distance;
	    float yOffset = sin(angle) * distance;
	    float alpha = ofMap(distance, minRadius, maxRadius, 50, 0);  	    ofSetColor(255, alpha);
	    ofDrawLine(ofGetMouseX(), ofGetMouseY(), ofGetMouseX()+xOffset, ofGetMouseY()+yOffset);
	}
	
###画三角形
        int numTriangles = 10;
        int minOffset = 5;
        int maxOffset = 70;
        int alpha = 150;

        for (int t=0; t<numTriangles; ++t) {
            float offsetDistance = ofRandom(minOffset, maxOffset);

            ofVec2f mousePos(ofGetMouseX(), ofGetMouseY());

            // Define a triangle at the origin (0,0) that points to the right
            ofVec2f p1(0, 6.25);
            ofVec2f p2(25, 0);
            ofVec2f p3(0, -6.25);

            float rotation = ofRandom(360); // The rotate function uses degrees!
            p1.rotate(rotation);
            p2.rotate(rotation);
            p3.rotate(rotation);

            ofVec2f triangleOffset(offsetDistance, 0.0);
            triangleOffset.rotate(rotation);

            p1 += mousePos + triangleOffset;
            p2 += mousePos + triangleOffset;
            p3 += mousePos + triangleOffset;

            ofColor aqua(0, 252, 255, alpha);
            ofColor purple(198, 0, 205, alpha);
            ofColor inbetween = aqua.getLerped(purple, ofRandom(1.0));
            ofSetColor(inbetween);

            ofDrawTriangle(p1, p2, p3);



