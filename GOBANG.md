# Gobang
This is the first project which was accmoplished by **Ziyi Huang @paradox-hzy ,Junjia Wang and Zhuoyi Lu**, as a final assignment for Computer Science Overview in **Nankai University**.

```cpp
#include <iostream>
#include <ctime>
#include <cstring>
#include <cstdlib>
#include <windows.h>
#include<conio.h>
using namespace std;

//先手的棋子为-1，后手的棋子为1,没有子的地方为0
//参数还未确定

int board[7][7] = { 0 };   //当前棋盘
long long value[7][7] = { {0,0,0,0,0,0,0},{0,1,2,3,2,1,0},{0,2,4,5,4,2,0},{0,3,5,6,5,3,0},{0,2,4,5,4,2,0},{0,1,2,3,2,1,0},{0,0,0,0,0,0,0} };   //棋盘每个位置的初始权值
long long link[5] = {  0,2,4,1000,100000000 };   //对于我方已有1，2，3，4个棋子连接时的权值（前提是该行，列，对角线上没有敌方棋子）
long long broke[5] = { 0,2,4,100,100000 };   //对于敌方已有1，2，3，4个棋子时的情况进行破坏的权值（前提是该行，列，对角线没有己方棋子）
long long ti;   //计算机下棋速度参数
char ch[11][4] = { "┌","┬","┐","├","┼","┤","└","┴","┘","○","●" };
char star[6][6];

char dr;

void color(int c)
{
	SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), c);        //更改文字颜色
	return;

}

void usetime()
{
	int k = 1;
	for (int i = 1; i <= ti; i++)
	{
		k = (k + i) % 5;
	}
}

void AItime()
{
	printf(
		"┌———————————————————————————————┐\n\n"
		"  Please select the speed of the AI chess dropping(1, 2, 3, 4)\n\n"
		"└———————————————————————————————┘\n"
	);
	//cout << "Please select the speed of the AI chess dropping(1,2,3,4)" << endl;
	char w;
	cin >> w;
	if (w == '1') ti = 300000000;
	else if (w == '2') ti = 200000000;
	else if (w == '3') ti = 100000000;
	else if (w == '4') ti = 50000000;
	else
	{
		Beep(300, 400); Beep(600, 400); Beep(600, 400);
		cout << "Our AI played fast just like DEEP BULE. That is the cost of cheating on AI" << endl;

		ti = 0;
	}
}

void p1()
{
	printf(
		"┌———————————————————————————————┐\n\n"

	);
}

void p2()
{
	printf(

		"\n\n"
		"└———————————————————————————————┘\n"
	);
}

long long getValue(int i, int j)   //计算每个点的权值
{
	long long ans = value[i][j];


	int own = 0;
	for (int t = 1; t <= 5; t++)
	{
		if (board[t][j] == -1) own++;
		else if (board[t][j] == 1) { own = 0; break; }//行寻找我方棋子
	}
	ans += link[own];

	own = 0;
	for (int t = 1; t <= 5; t++)
	{
		if (board[i][t] == -1) own++;
		else if (board[i][t] == 1) { own = 0; break; }//列寻找我方棋子
	}
	ans += link[own];

	own = 0;
	if (i == j)
	{
		for (int t = 1; t <= 5; t++)
		{
			if (board[t][t] == -1) own++;
			else if (board[t][t] == 1) { own = 0; break; }//一条对角线寻找我方棋子
		}
		ans += link[own];
	}

	own = 0;
	if (i == 6 - j)
	{
		for (int t = 1; t <= 5; t++)
		{
			if (board[t][6 - t] == -1) own++;
			else if (board[t][6 - t] == 1) { own = 0; break; }//另一条对角线寻找我方棋子
		}
		ans += link[own];
	}


	int rival = 0;
	for (int t = 1; t <= 5; t++)
	{
		if (board[t][j] == 1) rival++;
		else if (board[t][j] == -1) { rival = 0; break; }//敌方会进行和我们相类似的操作
	}
	ans += broke[rival];

	rival = 0;
	for (int t = 1; t <= 5; t++)
	{
		if (board[i][t] == 1) rival++;
		else if (board[i][t] == -1) { rival = 0; break; }
	}
	ans += broke[rival];

	rival = 0;
	if (i == j)
	{
		for (int t = 1; t <= 5; t++)
		{
			if (board[t][t] == 1) rival++;
			else if (board[t][t] == -1) { rival = 0; break; }
		}
		ans += broke[rival];
	}

	rival = 0;
	if (i == 6 - j)
	{
		for (int t = 1; t <= 5; t++)
		{
			if (board[t][6 - t] == 1) rival++;
			else if (board[t][6 - t] == -1) { rival = 0; break; }
		}
		ans += broke[rival];
	}

	return ans;
}

void AI(int k)   //困难人机
{
	cout << "AI's turn：" << endl;
	usetime();
	int best_i = 0, best_j = 0;
	long long best = 0;
	for (int i = 1; i <= 5; i++)
		for (int j = 1; j <= 5; j++)
		{
			if (board[i][j] != 0) continue;
			else
			{
				long long v = getValue(i, j);
				if (v > best)
				{
					best = v;
					best_i = i; best_j = j;
				}
			}
		}
	board[best_i][best_j] = k;
}

void zAI(int k)   //普通人机
{
	cout << "AI's turn：" << endl;
	usetime();
	int best_i = 0, best_j = 0;
	long long best = 0;
	for (int i = 1; i <= 5; i++)
		for (int j = 1; j <= 5; j++)
		{
			if (board[i][j] != 0) continue;
			else
			{
				long long v = getValue(i, j);
				if (v > best)
				{
					best = v;
					best_i = i; best_j = j;
				}
			}
		}
	if (best >= 100000) board[best_i][best_j] = k;
	else
	{
		srand(time(NULL));
		while (true)
		{
			int i = (rand()) % 5 + 1, j = (rand()) % 5 + 1;
			if (board[i][j] != 0) continue;
			else { board[i][j] = k; break; }
		}
	}
}

void zzAI(int k)   //简单人机
{
	srand(time(NULL));
	cout << "AI's turn：" << endl;
	usetime();
	while (true)
	{
		int i = (rand()) % 5 + 1, j = (rand()) % 5 + 1;
		if (board[i][j] != 0) continue;
		else { board[i][j] = k; break; }
	}
}


void work(int k)   //玩家落子（修改棋盘）
{
	void print();
	int a, b;
	a = b = 3;
	std:: cout << "please drop your chess：" << endl;
	char i, j;
	int f1, f2;
	f1 = f2 = 3;
loop:;
	dr= _getch();
	while (dr == 'w' || dr == 's' || dr == 'd' || dr == 'a') {
		if (dr == 'w') {
			f1++;
			if (f1 == 6) { f1--; goto loop; }
			star[a][b] = ' ';
			star[a - 1][b] = '*';
			a = a - 1;
		
		}
		if (dr == 's') {
			f1--;
			if (f1 == 0) { f1++; goto loop; }
			star[a][b] = ' ';
			star[a +1][b] = '*';
			a = a + 1;
			
			
		}
		if (dr == 'a') {
			f2--;
			if (f2 == 0) { f2++; goto loop; }
			star[a][b] = ' ';
			star[a][b-1] = '*';
			b = b - 1;
			
		}
		if (dr == 'd') {
			f2++;
			if (f2 == 6) { f2--;goto loop; }
			star[a][b] = ' ';
			star[a][b+1] = '*';
			b = b + 1;
			
		}
		print();
		dr =_getch();
	}
	
	i = a+48;
	j = b+48;
		if (i < '1' || i>'5' || j < '1' || j>'5' || board[i - 48][j - 48] != 0)
		{
			Beep(300, 400); Beep(600, 400); Beep(600, 400);
			printf("Don’t try to deceive the computer, we will always treat customers as mentally retarded, so please drop cheese again：\n");
			memset(star, ' ', sizeof(star));
			work(k);
		}
		else
		{
			star[a][b] = ' ';
			board[i - 48][j - 48] = k;
			return;
		}
		
	
}



void print()
{
	system("cls");
	char p_board[7][7];
	for (int i = 1; i <= 5; i++)
		for (int j = 1; j <= 5; j++)
		{
			if (board[i][j] == 1) p_board[i][j] = 'X';
			else if (board[i][j] == -1) p_board[i][j] = 'O';
			else if (board[i][j] == 0) p_board[i][j] = ' ';
		}
	printf("+---+---+---+---+---+\n"
		"| %c%c| %c%c| %c%c| %c%c| %c%c|\n"
		"+---+---+---+---+---+\n"
		"| %c%c| %c%c| %c%c| %c%c| %c%c|\n"
		"+---+---+---+---+---+\n"
		"| %c%c| %c%c| %c%c| %c%c| %c%c|\n"
		"+---+---+---+---+---+\n"
		"| %c%c| %c%c| %c%c| %c%c| %c%c|\n"
		"+---+---+---+---+---+\n"
		"| %c%c| %c%c| %c%c| %c%c| %c%c|\n"
		"+---+---+---+---+---+\n",
		p_board[1][1], star[1][1], p_board[1][2], star[1][2], p_board[1][3], star[1][3], p_board[1][4], star[1][4], p_board[1][5], star[1][5],
		p_board[2][1], star[2][1], p_board[2][2], star[2][2], p_board[2][3], star[2][3], p_board[2][4], star[2][4], p_board[2][5], star[2][5],
		p_board[3][1], star[3][1], p_board[3][2], star[3][2], p_board[3][3], star[3][3], p_board[3][4], star[3][4], p_board[3][5], star[3][5],
		p_board[4][1], star[4][1], p_board[4][2], star[4][2], p_board[4][3], star[4][3], p_board[4][4], star[4][4], p_board[4][5], star[4][5],
		p_board[5][1], star[5][1], p_board[5][2], star[5][2], p_board[5][3], star[5][3], p_board[5][4], star[5][4], p_board[5][5], star[5][5]);
	cout << endl;
}


int check()
{
	int win = 0;
	for (int i = 1; i <= 5; i++)
	{
		if (board[i][1] == 0) continue;
		int j;
		for (j = 2; j <= 5; j++)
		{
			if (board[i][j] != board[i][1]) break;
		}
		if (j == 6) { win = board[i][1]; return win; }
	}

	for (int j = 1; j <= 5; j++)
	{
		if (board[1][j] == 0) continue;
		int i;
		for (i = 2; i <= 5; i++)
		{
			if (board[i][j] != board[1][j]) break;
		}
		if (i == 6) { win = board[1][j]; return win; }
	}

	if (board[1][1] != 0)
	{
		int t;
		for (t = 2; t <= 5; t++)
		{
			if (board[t][t] != board[1][1]) break;
		}
		if (t == 6) { win = board[1][1]; return win; }
	}

	if (board[1][5] != 0)
	{
		int t;
		for (t = 2; t <= 5; t++)
		{
			if (board[t][6 - t] != board[1][5]) break;
		}
		if (t == 6) { win = board[1][5]; return win; }
	}
	int full = 1;
	for (int i = 1; i <= 5; i++)
		for (int j = 1; j <= 5; j++)
		{
			if (board[i][j] == 0) { full = 0; goto next; }
		}
next:;
	if (full == 1) return 100;
	return win;
}  //检测胜负情况


int choose1()
{
	char x;
	cin >> x;
	int t = x - 48;
	if (x != '0' && x != '1' && x != '2')
	{
		Beep(600, 400); Beep(600, 400); Beep(600, 400);
		p1();
		cout << "Don’t try to deceive the computer, we will always treat customers as mentally retarded, so please drop cheese again：" << endl;
		p2();
		t = choose1();
	}
	return t;
}

int choose2()
{
	char x;
	cin >> x;
	int t = x - 48;
	if (x != '0' && x != '1')
	{
		Beep(600, 400); Beep(600, 400); Beep(600, 400);
		p1();
		cout << "Don’t try to deceive the computer, we will always treat customers as mentally retarded, so please drop cheese again：" << endl;
		p2();
		t = choose2();
	}
	return t;
}
int main()
{
newgame:;
	memset(board, 0, sizeof(board));
	memset(star, ' ', sizeof(star));
	//star[3][3] = { '*' };
	printf(
		"┌———————————————————————————————┐\n"
		"\n   Warm welcome to you for coming GOBANG!\n\n   This game made by genuis software engineerings.\n\n   Hope you will enjoy it\n\n"
		"   Game tips: X for the first hand and O for the second hand    \n\n"

		"   Using 'wasd' to contol your cursor and drop your chess \n"
		"\n"
		"└———————————————————————————————┘\n"
	);
	getchar();
	system("cls");
	AItime();
	//print();
	printf(
		"┌———————————————————————————————┐\n\n"
		"  Man - Machine Battle select 0\n  Man - Man Battle select 1\n  Machine - Machine Battle select 2"
		"\n\n"
		"└———————————————————————————————┘\n"
	);
	getchar();
	//cout << "Man-Machine Battle select 0, Man-Man Battle select 1, Machine-Machine Battle select 2：" << endl;
	int x = choose1();
	print();
	if (x == 0)
	{
		printf(
			"┌———————————————————————————————┐\n\n"
			" Select 0 for first hand and 1 for last hand"
			"\n\n"
			"└———————————————————————————————┘\n"
		);
		//cout << "Select 0 for first hand and 1 for last hand：" << endl;
		int choose = choose2();
		printf(
			"┌———————————————————————————————┐\n\n"
			" Difficulty select: simple select 0, medium select 1, difficult select 2"
			"\n\n"
			"└———————————————————————————————┘\n"
		);
		//cout << "Difficulty select: simple select 0, medium select 1, difficult select 2：" << endl;
		int y = choose1();
		if (choose == 0)
		{
			while (true)
			{
				work(1);
				print();
				if (check() == 1) { Beep(300, 400); Beep(600, 400); Beep(600, 400); p1(); cout << "  666 You are so genius!" << endl; p2(); break; }
				if (check() == 100) { Beep(600, 400); Beep(600, 400); Beep(600, 400); p1(); cout << "  Congratulations on your tie with Artificial Intelligence" << endl; p2(); break; }
				if (y == 1) zAI(-1);
				else if (y == 0) zzAI(-1);
				else if (y == 2) AI(-1);
				print();
				if (check() == -1) { Beep(500, 400); Beep(500, 400); Beep(500, 400); p1(); cout << "Rethink deeply about what you did" << endl; p2(); break; }
				if (check() == 100) { Beep(700, 200); Beep(700, 200); Beep(700, 200); p1(); cout << "Congratulations on your tie with Artificial Intelligence" << endl; p2(); break; }
			}
		}
		if (choose == 1)
		{
			while (true)
			{
				if (y == 1) zAI(1);
				else if (y == 0) zzAI(1);
				else if (y == 2) AI(1);
				print();
				if (check() == 1) { Beep(500, 400); Beep(500, 400); Beep(500, 400); p1();  cout << "Rethink deeply about what you did" << endl; p2(); break; }
				if (check() == 100) { Beep(700, 200); Beep(700, 200); Beep(700, 200); p1(); cout << "Congratulations on your tie with Artificial Intelligence" << endl; p2(); break; }
				work(-1);
				print();
				if (check() == -1) { Beep(300, 400); Beep(600, 400); Beep(600, 400); p1(); cout << "dalao tql!" << endl; p2(); break; }
				if (check() == 100) { Beep(300, 400); Beep(600, 400); Beep(600, 400); p1(); cout << "Congratulations on your tie with Artificial Intelligence" << endl; p2(); break; }
			}
		}
	}
	if (x == 1)
	{
		while (true)
		{
			work(1);
			print();
			if (check() == 1) { p1(); cout << "First hand won!" << endl; p2(); break; }
			if (check() == 100) { p1(); cout << "Congratulations on your tie" << endl; p2(); break; }
			work(-1);
			print();
			if (check() == -1) { p1(); cout << "Second hand won!" << endl; p2(); break; }
			if (check() == 100) { p1(); cout << "Congratulations on your tie" << endl; p2(); break; }
		}
	}
	if (x == 2)
	{
		p1();
		cout << "  Why do you want to watch two artificially retarded fights?" << endl;
		p2();
		getchar();
		system("cls");
		p1();
		cout << "  Choose the first artificially retarded difficulty: \n\n  simple select 0,\n  medium select 1,\n  difficult select 2：" << endl;
		p2();
		int x1 = choose1();

		system("cls");

		p1();
		cout << "  Choose the first artificially retarded difficulty: \n\n  simple select 0,\n  medium select 1,\n  difficult select 2：" << endl;
		p2();
		int x2 = choose1();
		system("cls");


		while (true)
		{
			if (x1 == 1) zAI(1);
			else if (x1 == 0) zzAI(1);
			else if (x1 == 2) AI(1);
			print();
			if (check() == 1) { Beep(300, 400); Beep(600, 400); Beep(600, 400);  p1(); cout << "The first artificially retarded won" << endl; p2(); break; }
			if (check() == 100) { Beep(300, 400); Beep(600, 400); Beep(600, 400); p1();  cout << "ALL right.A tie. Now satisfied？" << endl; p2(); break; }
			if (x2 == 1) zAI(-1);
			else if (x2 == 0) zzAI(-1);
			else if (x2 == 2) AI(-1);
			print();
			if (check() == -1) { Beep(300, 400); Beep(600, 400); Beep(600, 400); p1();  cout << "The second artificially retarded won" << endl; p2(); break; }
			if (check() == 100) { Beep(300, 400); Beep(600, 400); Beep(600, 400); p1(); cout << "ALL right.A tie. Now satisfied?" << endl; p2(); break; }
		}
	}
	p1();
	cout << "Another battle? yes(1), no(0)" << endl; p2();
	int is_n = choose2();
	if (is_n == 1) goto newgame;
	else
	{
		p1();
		cout << "Thank you for your participating!" << endl;
		p2();
		return 0;
	}
}


