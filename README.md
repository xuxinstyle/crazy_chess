# 疯狂战棋题解
## 题目
最近编程猫在玩一款回合制战棋游戏，这款战棋游戏最神奇的地方在于采用的居然是双人合作模式。

棋局的规模正好是一个正方形 n * n，游戏对棋局的所有格子（注意是格子，不是十字交叉点）进行编号，如第一行的格子的编号为从1到n，第二行的格子的编号为从n + 1到2n，如此推算，最后一行的编号为n * n - n + 1到n * n。

甲乙双方各持一枚棋子，均从最左上角的格子出发（注意是格子，不是十字交叉点）。每回合到来，甲和乙都可以分别按照程序指定给他们的格子编号序列走到下一步或者弃权本回合（即可以选择继续走序列中下一个编号，或者不走，允许无限期停留）。对于甲和乙，程序会分别指定不同的格子编号序列，且同一序列中编号不会重复。

甲和乙必须按照游戏程序给定的编号序列一个个往下走，不能跳过某一个编号，也不能回退到之前的编号。

甲和乙的棋子只要在同一个格子内，那个格子就会被点亮（点亮后不会被熄灭），而游戏评分就是被点亮的格子总数。只要甲和乙都同意，游戏随时都可以被结束。

假设甲和乙知道对方和自己要走的格子编号序列，且甲和乙足够聪明，会在游戏拿不到更高分数时直接结束游戏，请问甲和乙最终最多能拿多少分？

## 输入及其规模
第一行将输入且仅输入一行，这一行仅包含一个数字，用于表示游戏场数t (1 <= t <= 10)

对于每一场游戏的数据，总共包含3行：

第1行包含3个数字，分别代表n, p, q (2 <= n <= 255, 1 <= p, q <= n * n)
第2行包含p + 1个数字，用于表示甲要走的编号序列，第一个数字当然是1，随后紧跟着p个数用于表示甲接下来的可以走的编号序列
第3行包含q + 1个数字，用于表示乙要走的编号序列
完成t场游戏后，算法程序需要立刻退出，否则很可能会被判定为超时。

## 输出
对每一场游戏数据，输出一行，每一行两个部分，第一个部分输出Round i，i用于表示这是第几局游戏，随后紧跟着一个冒号和空格，然后输出答案a，用于表示甲和乙能获得的总分。

## 限制
时间限制：2000ms 空间限制：64MB（�保证至少有16MB的连续内存） 编码限制：UTF-8

## 例子
### 输入
1
4 8 9
1 4 7 6 8 2 3 10 15
1 4 7 15 6 5 8 3 10 12
### 输出
Round 1: 7

## 解析：
看到该题第一个反应，该题是有关求最长公共子序列的问题，即使用动态规划
当甲乙同时站在同一个位置时灯亮，就相当于当其序列中的响应位置的数字相同时则亮灯数+1
令甲乙序列从后往前比较，令甲的序列为Ai（i=0,1,2...p-1），乙的序列为Bi（i=0,1,2...q-1）
令最长公共子序列的长度为LCS(p,q)。
1.若Ap-1==Bq-1 则LCS(p,q)=LCS(p-1,q-1)+1;注：若Ap-1==Bp-1则一定属于最长公共子序列的一部分
2.否则Ap-1!=Bq-1 则LCS(p,q)=max(LCS(p-1,q),LCS(p,q-1));注：若Ap-1!=Bp-1，则LCS(p,q)为两个子序列其中一个去掉最后一个元素后最大的公共子序列长度
赋上java源码为：
import java.util.Scanner;
//最大公共子序列问题
public class Main {
	public static void main(String[] args) {
		Scanner sc=new Scanner(System.in);
		int t=sc.nextInt();
		for (int i = 0; i < t; i++) {
			int n = sc.nextInt();
			int p = sc.nextInt();
			int q = sc.nextInt();
			int first[]=new int[p+1];
			int second[]=new int[q+1];
			for (int j = 0; j < first.length; j++) {
				first[j]=sc.nextInt();
			}
			for (int j = 0; j < second.length; j++) {
				second[j]=sc.nextInt();
			}
			
			int visit[][]=new int[first.length+1][second.length+1];
			for (int j = 1; j < first.length + 1 ; j++) {
				for (int k = 1; k < second.length + 1; k++) {
					if(first[j-1]==second[k-1]){
						visit[j][k]=visit[j-1][k-1]+1;
					}else if(visit[j-1][k]>visit[j][k-1]){
						visit[j][k]=visit[j-1][k];
					}else{
						visit[j][k]=visit[j][k-1];
					}
				}
			}
			System.out.println("Round "+(i+1)+": "+visit[first.length][second.length]);
		}
		sc.close();
	}
}
