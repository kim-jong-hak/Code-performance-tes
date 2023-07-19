
## 11724번 문제를 해결하는 알고리즘이며 어떤 것이 가장빠를까?

나는 배열+list가 가장 빠르지 않을까? 생각한다.
배열은 list에 나오는 add() , get() 메서드를 처리하지 않고, 바로 인덱스를 넣어 실행하니까..

근데 ..이번에 내가 알고 있던 개발자 유튜버가

2차원 list를 선호하시고 접근이 빠를 수 있다는데……. 
로직이 엄청 복잡한 경우에 효과는 있을꺼 같지만.. 일단 간단한 예제로 실험해 보자!



### 2차원 리스트를 이용한 로직

```java
import java.util.*;
import java.io.*;

public class dfs {

    static ArrayList <ArrayList<Integer>>list=new ArrayList<>();
    static  boolean chaking[];
    public static void main(String[] ares)throws IOException
    {
        long startTime = System.currentTimeMillis();//시간 측정
        BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st=new StringTokenizer( br.readLine()," ");
        int node=Integer.parseInt(st.nextToken());
        int line=Integer.parseInt(st.nextToken());

        chaking=new boolean[node+1];// 인덱스는 0부터 시작인데 6개의 자리를 사용해야 하니까.. +1

        for(int i=0;i<node+1;i++)// 위와 마찬가지
         list.add(new ArrayList<>());

        for(int i=0;i<line;i++)// 입력수를 맞추기 위해서 line사용
        {
            st =new StringTokenizer(br.readLine()," ");
            int x=Integer.parseInt(st.nextToken());
            int b=Integer.parseInt(st.nextToken());
             list.get(x).add(b);
             list.get(b).add(x);//문제에서 방향을 주지 않았으니까 양방향으로 한다.

        }

        int count=0;
        for(int i=1;i<node+1;i++)
        {
           if(chaking[i]==false)
           {
               count++;
               DFS(i);
           }
        }
                   System.out.println(count);
// 시간 측정 종료
        long endTime = System.currentTimeMillis();

        // 실행 시간 계산
        long executionTime = endTime - startTime;
        System.out.println("Execution Time: " + executionTime + " milliseconds");

    }
    static void DFS(int i)
    {

        if (chaking[i])
            return;

        chaking[i]=true;

        for(int t:list.get(i))
        {
            if(chaking[t]==true)
                continue;
            DFS(t);

        }
    }

}

===================== 입력 값 ====================
6 5
1 2
2 5
5 1
3 4
4 6
```


### 배열+list 이용한 로직

```java
import java.io.*;
import java.util.*;
public class Main {

    static ArrayList <Integer>list[];
    static  boolean chaking[];
    public static void main(String[] ares)throws IOException
    {
         long startTime = System.currentTimeMillis();//시간 측정
        BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st=new StringTokenizer( br.readLine()," ");
        int node=Integer.parseInt(st.nextToken());
        int line=Integer.parseInt(st.nextToken());

        chaking=new boolean[node+1];// 인덱스는 0부터 시작인데 6개의 자리를 사용해야 하니까.. +1
        list=new ArrayList[node+1];// 마찬가지

        for(int i=0;i<node+1;i++)// 위와 마찬가지
         list[i]=new ArrayList<>();

        for(int i=0;i<line;i++)// 입력수를 맞추기 위해서 line사용
        {
            st =new StringTokenizer(br.readLine()," ");
            int x=Integer.parseInt(st.nextToken());
            int b=Integer.parseInt(st.nextToken());
             list[x].add(b);
             list[b].add(x);//문제에서 방향을 주지 않았으니까 양방향으로 한다.

        }
        chaking[0]=true;

        int count=0;
        for(int i=0;i<node+1;i++)
        {
           if(chaking[i]==false)
           {
               count++;
               DFS(i);
           }
        }
System.out.println(count);
// 시간 측정 종료
        long endTime = System.currentTimeMillis();

        // 실행 시간 계산
        long executionTime = endTime - startTime;
        System.out.println("Execution Time: " + executionTime + " milliseconds");

    }
    static void DFS(int i)
    {

        if (chaking[i])
            return;

        chaking[i]=true;

        for(int t:list[i])
        {
            if(chaking[t]==true)
                continue;
            DFS(t);

        }
    }

}

===================== 입력 값 ====================
6 5
1 2
2 5
5 1
3 4
4 6
```



| 2차원 리스트 [시간 측정 결과] |
|:--:|
|1번째 : Execution Time: 1707 milliseconds|
|2번째 : Execution Time: 5534 milliseconds|
|3번째 : Execution Time: 2977 milliseconds|
|4번째 : Execution Time: 2444 milliseconds|


--------

| 배열+list [시간 측정 결과] |
|:--:|
|1번째 : Execution Time: 10118 milliseconds|
|2번째 : Execution Time: 1623 milliseconds|
|3번째 : Execution Time: 1123 milliseconds|
|4번째 : Execution Time: 1984 milliseconds|



<aside>
📌 결론: 배열+list가 평균적으로 빠른거 같지만.. 중간 중간 크게 시간이 많이 걸리는게 보이니까, 측정환경이 잘못된건지 의심쓰럽다. 

</aside>
