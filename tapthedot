#include "macros.h"
#include "ulk.h"

int main(void) PROGRAM_ENTRY;  //Marks the begining of program execution.
void display_frame();
void clear_display();

vuint32 *BASE = (vuint32 *)0x80500000;
/*Touch pad controller*/
struct PIXEL          //This structure includes the parameter that are considered as the co-ordinate on the control panel.
{
	unsigned int x;
	unsigned int y;
};
extern struct PIXEL pixel;
extern struct PIXEL ulk_proc_touch_spi_enable(void);  //print x and y co-ordinate. Serial Peripheral Interface
extern void ulk_proc_touch_spi_disable(void);
extern struct PIXEL ulk_proc_touch_spi_poll(void);  //returns x and y co-ordinate values to the main function.

char boxes[11];

/*DISPLAY 4X3 BOARD ON TOUCH LCD*/
void display_frame(void)
{
    uint32 row, col, offset;

    //horizontal lines
	for (row=15;row<16;row++)
		for (col=15;col<295;col++)
		{
			offset=row*320+col;
			*(BASE+offset)=0x0000FF;
		}
	for (row=85;row<86;row++)
		for (col=15;col<295;col++)
		{
			offset=row*320+col;
			*(BASE+offset)=0x0000FF;
		}
	for (row=155;row<156;row++)
		for (col=15;col<295;col++)
		{
			offset=row*320+col;
			*(BASE+offset)=0x0000FF;
		}
	for (row=225;row<226;row++)
		for (col=15;col<295;col++)
		{
			offset=row*320+col;
			*(BASE+offset)=0x0000FF;
		}
	//vertical lines
	for (row=15;row<225;row++)
	{
		offset=row*320+15;
		*(BASE+offset)=0x0000FF;
	}
	for (row=15;row<225;row++)
	{
		offset=row*320+85;
		*(BASE+offset)=0x0000FF;
	}
	for (row=15;row<225;row++)
	{
		offset=row*320+155;
		*(BASE+offset)=0x0000FF;
	}
	for (row=15;row<226;row++)
	{
		offset=row*320+225;
		*(BASE+offset)=0x0000FF;
	}
    for (row=15;row<226;row++)
	{
		offset=row*320+295;
		*(BASE+offset)=0x0000FF;
	}
}

/*CLEAR ALL, MEMORY, INITIALIZE, CLEAR SCREEN*/
void clear_display()
{
	uint32 row,col,offset;
	for(row=0;row<240;row++)
		for(col=0;col<320;col++)
		{
			offset=row*320+col;
			*(BASE+offset)=0x000000;
		}
}


int touch_input()
{
	pixel = ulk_proc_touch_spi_enable();

	int count=0;
	while(count<1)
	{
		pixel = ulk_proc_touch_spi_poll();
		if (pixel.x!=0 && pixel.y!=0)
		{
			count++;
		}
		//ulk_proc_delay(ULK_MSEC(100));  //Includes specified amount of delay in calling function.

	}
	ulk_proc_touch_spi_disable();
	//ulk_proc_delay(ULK_MSEC(100));
	if(pixel.x>15&&pixel.x<85)
	{
		if(pixel.y>155&&pixel.y<225)
			return 8;
		else if(pixel.y>85&&pixel.y<155)
			return 4;
		else if(pixel.y>15&&pixel.y<85)
			return 0;
	}
	else if(pixel.x>85&&pixel.x<155)
	{
		if(pixel.y>155&&pixel.y<225)
			return 9;
		else if(pixel.y>85&&pixel.y<155)
			return 5;
		else if(pixel.y>15&&pixel.y<85)
			return 1;
	}
	else if(pixel.x>155&&pixel.x<225)
	{
		if(pixel.y>155&&pixel.y<225)
			return 10;
		else if(pixel.y>85&&pixel.y<155)
			return 6;
		else if(pixel.y>15&&pixel.y<85)
			return 2;
	}
	else if(pixel.x>225&&pixel.x<295)
	{
		if(pixel.y>155&&pixel.y<225)
			return 11;
		else if(pixel.y>85&&pixel.y<155)
			return 7;
		else if(pixel.y>15&&pixel.y<85)
			return 3;
	}
	else
		return 18;

	return 0;
}

void win_check(int ss)
{
	uint32 target;
	ulk_proc_clcd_init();  //Initialize LCD with default configurations.(Processor Interfaces)
	ulk_fpga_clcd_init();  //Initialize LCD with default configurations(FPGA INTERFACES).Field Programmable Gate Array
	ulk_proc_clcd_display_on();  //Switches on the display.
	ulk_fpga_clcd_display_on();  //Switches on the display.
	//ulk_cpanel_printf("Your score is %d",ss);
	ulk_fpga_clcd_display_string("Congratulations!!!");  //Displays a string.
	ulk_proc_delay(ULK_MSEC(15));
	ulk_fpga_clcd_display_string("Your final score is shown below.");
	ulk_fpga_7seg_led_disable();

}

int check_score(int tag,int d)
{
    if (tag==d)
        return 1;
    else
        return 0;
}


void display_o(int row,int col,int r)
{
    int i,j,x,y,offset;
    x = 25 + 70 * (r%4);
    y = 215 - 70 * (r/4);
    for(i = x; i<= x+50; i++)
    	for(j = y; j>= y-50; j--)
    		if(((i-row)*(i-row) + (j-col)*(j-col) < 625)){
    	        offset=(j)*320+(i);
    	        *(BASE+offset)=0xFF0000;
    		}
}

void display_r(int row,int col,int r)
{
    int i,j,x,y,offset;
    x = 25 + 70 * (r%4);
    y = 215 - 70 * (r/4);
    for(i = x; i<= x+50; i++)
    	for(j = y; j>= y-50; j--)
    		if(((i-row)*(i-row) + (j-col)*(j-col) < 625)){
    	        offset=(j)*320+(i);
    	        *(BASE+offset)=0xFF0000;
    		}
}

void display_g(int row,int col,int r)
{
    int i,j,x,y,offset;
    x = 25 + 70 * (r%4);
    y = 215 - 70 * (r/4);
    for(i = x; i<= x+50; i++)
    	for(j = y; j>= y-50; j--)
    		if(((i-row)*(i-row) + (j-col)*(j-col) < 625)){
    	        offset=(j)*320+(i);
    	        *(BASE+offset)=0x00FF00;
    		}
}

void display_b(int row,int col,int r)
{
    int i,j,x,y,offset;
    x = 25 + 70 * (r%4);
    y = 215 - 70 * (r/4);
    for(i = x; i<= x+50; i++)
    	for(j = y; j>= y-50; j--)
    		if(((i-row)*(i-row) + (j-col)*(j-col) < 625)){
    	        offset=(j)*320+(i);
    	        *(BASE+offset)=0x0000FF;
    		}
}

int getranddot(int r)
{
    int  row, col;

    //srand (time(NULL));
    //r = rand() % 12;

    r=(r*7)%12;
    row = 50 + 70 * (r%4);
    col = 190 - 70 * (r/4);
    display_o(row, col, r);
    return r;
}

void reset(int flag)
{
	ulk_proc_clcd_init();
	ulk_fpga_clcd_init();
	int target,z=0,tt;
	clear_display();
	display_frame();
	ulk_proc_clcd_display_on();
	ulk_fpga_clcd_display_on();
	ulk_fpga_7seg_led_enable();
	int d;
	int sco=0,count=0;
	ulk_fpga_clcd_display_string("Tap the dot");
	ulk_proc_enable_rtc();

	// Define the RTC structure
	struct rtc_struct rtc_struct1;

	// Define the time and Date value in the structure
	rtc_struct1.tm_sec = 00;
	ulk_proc_rtc_set_time (&rtc_struct1);
	//while(over(z)==1)
	if (flag==1)
		tt=10;
	else if (flag==2)
		tt=20;
	else 
		tt=30;
	while(rtc_struct1.tm_sec<=tt)
	{
		clear_display();
		display_frame();
		d=getranddot(z++);
		target=touch_input();
		if (check_score(target,d)==1)
		{
			count++;
			sco = count + 6 * (count/10);
			ulk_fpga_7seg_led_write(sco);
		}
		ulk_proc_rtc_get_time (&rtc_struct1);
	}
	clear_display();
	ulk_proc_delay(ULK_MSEC(200));
	win_check(sco);
	ulk_fpga_clcd_display_off ();
	//ulk_fpga_7seg_led_disable();
}

int options()
{
	ulk_proc_clcd_init();
	ulk_fpga_clcd_init();
	ulk_fpga_7seg_led_enable();
	int flag,p=1;
	ulk_proc_clcd_display_on();
	ulk_fpga_clcd_display_on();
	ulk_cpanel_printf("Choose(Tap) any one of the dot\n1)Red=10 seconds\n2)Green=20 seconds\n3)Blue=30seconds\n");
	clear_display();
	display_options();
	int touch;
	ulk_fpga_clcd_display_string("WELCOME");
	while(p!=12)
	{
	touch=touch_input();
	if (touch==5)
	{flag=1; p=12;}
	else if (touch==6)
	{flag=2; p=12;}
	else if (touch==7)
	{flag=3; p=12;}
	else
	p=1;
		}
	clear_display();
	ulk_cpanel_printf("Thank you. Your game starts now...");
	ulk_proc_delay(ULK_MSEC(200));
	return flag;
}

void display_options()
{
    /*int  row, col,r;
    for(r=5;r<8;r++)
    {
    row = 50 + 70 * (r%4);
    col = 190 - 70 * (r/4);
    display_o(row, col, r);
    }
    */
	int  row, col;
	row = 50 + 70 * (5%4);
	col = 190 - 70 * (5/4);
	display_r(row, col, 5);
	row = 50 + 70 * (6%4);
	col = 190 - 70 * (6/4);
	display_g(row, col, 6);
	row = 50 + 70 * (7%4);
	col = 190 - 70 * (7/4);
	display_b(row, col, 7);
}

int main()
{
   int f;
   f=options();
   reset(f);
   return 1;
}

