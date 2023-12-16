# (백준 11724번 문제)
### 배열과 리스트 혼용 vs 2차원 리스트: 성능 비교
저는 문제를 풀다가 문득 생각이 들었습니다.
배열과 리스트를 함께 활용하는 방법과 2차원 리스트 중 어떤 것이 성능상 더 우수한지에 대한 고민이 들기 시작했죠

누구는 의미없는 호기심이라고 생각할 수 있지만, 저는 알아내고 싶은 과제였죠 

저의 개인적인 생각으로는 배열과 리스트를 결합한 방식이 가장 효율적일 것으로 예상합니다. 
배열은 add()나 get() 메서드를 거치지 않고 인덱스를 통한 직접 접근이 가능하므로 처리 속도가 빠를 것으로 예상하고 있죠

그러나, 최근에 접한 개발자 유튜버의 의견에 따르면 2차원 리스트를 선호하며, 빠른 접근이 가능하다고 주장합니다..
일단은 단순한 예제로 실험을 통해 확인해보고자 합니다."



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
📌 결론: 배열+list가 평균적으로 빠른거 같지만.. 중간 중간 크게 시간이 많이 걸리는게 보이니까, 측정환경이 잘못된건지 의심쓰럽군요

</aside>
