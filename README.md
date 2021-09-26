# 2048

#include<iostream>
#include<ctime>
#include<iomanip>
#include<cstdlib>
using namespace std;

int board[4][4];
int dirLine[]={1,0,-1,0};
int dirCol[]={0,1,0,-1};

pair<int,int> generateUnoppupiedPos(){
	int occu=1,line,column;
	while (occu){
		 line=rand() % 4;
		 column=rand() % 4;
		if(board[line][column]==0){
			occu=0;
		}
	}
	return make_pair(line,column);
}
void addPiece(){
	pair<int,int> pos=generateUnoppupiedPos();
	board[pos.first][pos.second]=2;	
}

void newGame(){
	for(int i=0;i<4;++i){
		for(int j=0;j<4;++j){
			board[i][j]=0;
		} 
	}
	addPiece();
}
void printUI(){
	system("cls");
		for(int i=0;i<4;++i){
		for(int j=0;j<4;++j){
			if(board[i][j]==0) { 
				cout<<setw(4)<<".";
			}
			else { 
				cout<<setw(4)<<board[i][j];
			}
		}
		cout<<"\n"; 
	}
	cout<<"n: new game, w: up, s: down, d: right, a: left, q: quit\n";
}

bool canDoMove(int line,int column,int nextLine,int nextColumn){
	if(nextLine<0 || nextColumn<0 || nextLine>=4 || nextColumn>=4 || board[line][column]!=board[nextLine][nextColumn] && board[nextLine][nextColumn]!=0){
		return false;
	}
	return true;
}
void applyMove(int direction){
	int startLine=0,startColumn=0,linestep=1,columnstep=1;
	if(direction==0){
		startLine=3;
		linestep=-1;
	}
	if(direction==1){
		startColumn=3;
		columnstep=-1;
	}
	int movePossible,canaddPiece=0;
	do{
		movePossible=0;
		for(int i=startLine;i>=0 && i<4;i+=linestep){
			for(int j=startColumn;j>=0 && j<4;j+=columnstep){
				int nextI= i+dirLine[direction],nextJ=j+ dirCol[direction];
				if(board[i][j] && canDoMove(i,j,nextI,nextJ)){
					board[nextI][nextJ]+=board[i][j];
					board[i][j]=0;
					movePossible=canaddPiece=1;
				}
			}
		}
		printUI();
	} while(movePossible);
	if(canaddPiece){
		addPiece();
	}
	
}

int main(){
	srand(time(0));
	char commandTo[128];
	commandTo['s']=0;
	commandTo['d']=1;
	commandTo['w']=2;
	commandTo['a']=3;
	newGame();
	while(true){
		printUI();
		char command;
		cin>>command;
		if(command=='n'){
			newGame();
		}
		else if(command=='q'){
			break;
		}
		else{
			int currdir= commandTo[command];
			
			applyMove(currdir);
		}
	}
}
