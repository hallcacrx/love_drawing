# love_drawing
drawing


// draw heart
#include <stdio.h>
#include <math.h>
#include <Windows.h>
//#include <times.h>

float  f(float x, float y, float z) {
	float a = x * x + 9.0f / 4.0f * y * y + z * z - 1;
	return a*a*a - x*x*z*z*z - 9.0f / 80.0f*y*y*z*z*z;
}

float h(float x, float z)
{
	float y;
	for (y = 1.0f; y >= 0.0f; y -= 0.001f)
	{
		if (f(x, y, z) <= 0.0f)
		{
			return y;
		}
	}
	return 0.0f;
}

unsigned char tcharv[] = { '.',':','-','=','*','-','@' };


 void winSize(int lines_height, int cols_width)
 {
	 char cmd[100];
	if ((lines_height<1) || (lines_height>168) || (cols_width<13) || (cols_width>9001))
		{
		    printf("\nWarnning from winSize(): 1<=lines_height<=168  13<=cols_width<=9001 \n");
		    lines_height = 168;
		    cols_width = 9001;
		}
	  sprintf(cmd, "mode con cols=%d lines=%d", lines_height, cols_width);
	  system(cmd);
 }

int main()
{
	HANDLE hw = GetStdHandle(STD_OUTPUT_HANDLE);
	WORD wOldColorAttrs;
	CONSOLE_SCREEN_BUFFER_INFO csbiInfo;
	float z, x;
	unsigned char  strvv[512];

	system("color 2");

	
	winSize(140,40);

	printf("\n\t\t\t\t\t ��Ϧ���֣�����\n  ");

	printf("\n\t\t��ĸ\t\t\t\t&\t\t С����\n  ");

	//system("color 4");

	for (z = 1.5f; z > -1.5f; z -= 2*0.05f)
	{
		for (x = -1.15f; x < 1.15f; x += 2*0.025f) {
			float v = f(x, 0.0f, z);
			if (v <= 0.0f)
			{
				float y0 = h(x, z);
				float ny = 0.01f;
				float nx = h(x + ny, z) - y0;
				float nz = h(x, z + ny) - y0;
				//float nd = 1.0f / sqrtf(nx*nx + ny*ny + nz*nz);
				float nd = 1.0f / sqrtf(nx * nx + ny * ny + nz * nz);
				float d = (nx + ny - nz) * nd * 0.5f + 0.5f;
				//putchar(tcharv[(int)(d*5.0f)]);
				putchar(".:-=+*#%@"[(int)(d * 5.0f)]);
			}
			else
			{
				putchar(' ');
			}
		}

		for (x = -1.15f; x < 1.15f; x += 2 * 0.025f) {
			float v = f(x, 0.0f, z);
			if (v <= 0.0f)
			{
				float y0 = h(x, z);
				float ny = 0.01f;
				float nx = h(x + ny, z) - y0;
				float nz = h(x, z + ny) - y0;
				//float nd = 1.0f / sqrtf(nx*nx + ny*ny + nz*nz);
				float nd = 1.0f / sqrtf(nx * nx + ny * ny + nz * nz);
				float d = (nx + ny - nz) * nd * 0.5f + 0.5f;
				//putchar(tcharv[(int)(d*5.0f)]);
				putchar(".:-=+*#%@"[(int)(d * 5.0f)]);
			}
			else
			{
				putchar(' ');
			}
		}

		putchar('\n');
	}
	//SetConsoleTextAttribute(hw, FOREGROUND_RED | FOREGROUND_INTENSITY | BACKGROUND_BLUE);
	printf(" \t\t\t\t\t\t\t\t\t\t ���£�");

	getchar();
	//system("pause");
	//gets(strvv);
	//scanf("%s,", &strvv);

	printf("��"); Sleep(100);
	printf("С"); Sleep(100);
	printf("��^.^"); Sleep(100);
	printf("&");  Sleep(100);
	printf("��"); Sleep(100);
	printf("С"); Sleep(100);
	printf("��^��^"); Sleep(1000);

	
}



//
a=imread('1_20180820135107.jpg');
k=imread('timg.jpg');
k=imresize(k,0.1);
a(520:620-1,400:500,:)=k;
a=rgb2hsv(a);
a(:,:,1)=a(:,:,1)*0.4;
a=hsv2rgb(a);
figure,imshow(a);

imwrite(a,'a.bmp');

a=imread('1_20180820135107.jpg');
a(520:620-1,450:550,:)=k;
b=a;
for i=1:3
k1=a(:,:,i);
k1=fft(k1);
k1=ifft(conj(k1));
b(:,:,i)=k1;
end;
b=imrotate(b,180);
b=uint8(b);
a=b;
a=rgb2hsv(a);
a(:,:,1)=(1-a(:,:,1))*0.7;
a=hsv2rgb(a);
figure,imshow(a);
imwrite(a,'b.bmp');

% imshow(a);
% text(401,51,'??','fontsize',40,'color','red');
% picname='2.jpg';
% saveas(gcf,picname)

r=0.15;

a=imread('1.jpg');
b=imread('2.jpg');

a=imresize(a,r);
b=imresize(b,r);

[size1,size2,size3]=size(a);



stepall=10;

for i=1:stepall

    t=(i-1)*100*r;
    ao=ones(size1,size2*3,3);
    ao=uint8(ao*255);
    ao(1:size1,1+t:size2+t,:)=b;
    
    ao(1:size1,size2*2+1-t:size2*2+size2-t,:)=a;
    
    figure,imshow(ao);
    picname=[num2str(i) '.fig'];%????????i=1??picname=1.fig
    saveas(gcf,picname)

end

for i=1:stepall
    picname=[num2str(i) '.fig'];
    open(picname)
    frame=getframe(gcf);  
    im=frame2im(frame);%??gif????????index????  
    [I,map]=rgb2ind(im,20);          
    if i==1
        imwrite(I,map,'t.gif','gif', 'Loopcount',inf,'DelayTime',0.5);%????????
    elseif i==stepall
        imwrite(I,map,'t.gif','gif','WriteMode','append','DelayTime',0.5);
    else
        imwrite(I,map,'t.gif','gif','WriteMode','append','DelayTime',0.5);
    end;  
    close all
end
