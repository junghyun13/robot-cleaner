# robot-cleaner
# c
로봇청소기는 동서남북방향으로 청소가 가능하다. 로봇청소기가 작동한후에 멈출때가지 청소한 방의 수를 출력하는 프로그램을 작성해보자! 첫째줄, 가로 세로 둘째줄, 로봇시작위치와 방향 셋째줄부터 장소의 위치를 0과 1로 나타냄 0은 방이 존재 1은 벽이다.
로봇 청소기는 다음과 같이 작동한다.
1번: 현재 칸이 아직 청소되지 않은 경우, 현재 칸을 청소한다.
현재 칸의 주변 
4칸 중 청소되지 않은 빈 칸이 없는 경우,
-바라보는 방향을 유지한 채로 한 칸 후진할 수 있다면 한 칸 후진하고 1번으로 돌아간다.
-바라보는 방향의 뒤쪽 칸이 벽이라 후진할 수 없다면 작동을 멈춘다.
현재 칸의 주변 
4칸 중 청소되지 않은 빈 칸이 있는 경우,
-반시계 방향으로 90도 회전한다.
-바라보는 방향을 기준으로 앞쪽 칸이 청소되지 않은 빈 칸인 경우 한 칸 전진한 후, 1번으로 돌아간다.

#include <stdio.h>
#include <stdlib.h>
int n,m,r,c,d,i,j;
int room[100][100],visit[100][100];
int dx[4]={-1,0,1,0}; //북:0 동:1 남:2 서:3 
int dy[4]={0,1,0,-1};


int clean(){
	int cnt,re;
	int handle=d-1+((d-1<0)? 4:0);
	while(1){
		if(re>=4){//4방향을 모두 청소했다면  
			handle=d-2+((d-2<0)? 4:0); //그대로 후진  
			if(room[r+dy[handle]][c+dx[handle]]==1){
				break;}
			r+=dy[handle];
			c+=dx[handle];
			re=0;
			handle=d-1+((d-1<0)? 4:0);
			continue;}
		if(visit[r][c]==0){//청소한적이 없다면  
			visit[r][c]=1;
			cnt++;}
		int nx=c+dx[handle];//청소기의 방향을 반시계로 돌리기  
		int ny=r+dy[handle];
		if(nx<m && nx>=0 && ny<n && ny>=0){//room의 범위안에 있으면  
			if(visit[ny][nx]==1){//방문한적이 있다면  
				handle=handle-1+((handle-1<0)? 4:0);
				re++;
				continue;}
			if(room[ny][nx]==1){//벽이 존재해서  
				handle=handle-1+((handle-1<0)? 4:0); //방향돌리기  
				re++;
				continue;}
			c=nx; //갈수 있는 곳이라면 이동  
			r=ny;
			d=handle;	
			handle=d-1+((d-1<0)? 4:0);}
		else{ //갈수있는 범위를 벗어났다면?
		    handle=handle-1+((handle-1<0)? 4:0);
		    re++;
		    continue;}}
	return cnt;}
	
	
int solution(){
	int rt=0;
	rt=clean();
	return rt;}
	
	
int main(){
	scanf("%d %d",&n,&m); //n은 세로 m은 가로  
	scanf("%d %d %d",&r,&c,&d); //r번째줄 c번째 값이 처음 작동위치이고 d는 동서남북을 가르킴  
	for(i=0;i<n;i++){
		for(j=0;j<m;j++){
			scanf("%d",&room[i][j]);}}
	int cleanroom=solution();
	printf("로봇청소기가 청소한 방의 수: %d",cleanroom);
