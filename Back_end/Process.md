## 프로세스 

- 프로그램이 실행돼서 돌아가고 있는 상태 즉 컴퓨터가 어떤 일을 하고 있는 상태를 프로세스라고 한다

- 여러 프로세스를 함께 돌리는 작업은
동시적, 병렬적, 또는
이 둘의 혼합으로 이뤄진다
    
    - Concurrency(동시적)
    프로세스 하나가 이거 조금하고 이거 조금하고 이거 조금하고 이렇게 여러 작업을 돌아가면서 일부분씩 진행되는 것
    - Perallelusm(병렬적)
    프로세스 하나가에 코어 여러 개가 달려서 각각 동시에 작업들을 수행하는 것<br>
    
    듀얼코어, 쿼드코어, 옥타코어 이런 명칭이 붙는 멀티코어 프로세스가 달린 컴퓨터에서 할 수 있는 방식

## 스레드

- 한 프로세스 내에서도 여러 갈래의 작업들이 동시에 진행될 필요가 있음
<br>
이 갈래를 스레드라고 한다

- 프로세스들은 컴퓨터의 자원을 분할해서 쓰지만 스레드는 프로세스마다 주어진 전체 자원을 함께 사용한다
    - 속도와 효율 면에스는 낫겠지만 
    오류를 예상하고 방지해야 하기 때문에 스레드를 사용하는 프로그램은 코드를 짜기도, 디버깅을 해서 오류를 찾아내고 원인을 밝히기도 굉장히 까다로운 경우가 많다




```java
import java.util.Scanner;



public class RamenProgram 
{



	public static void main(String[] args) 
	{
		int num;
		Scanner input = new Scanner(System.in);
		
		System.out.println("라면 몇 개 끓일까요?");
		
		num = input.nextInt();
		
		System.out.println(num + "개 주문 완료! 조리시작!");
		try
		{
			RamenCook ramenCook = new RamenCook(num);
			new Thread(ramenCook,"A").start();
			new Thread(ramenCook,"B").start();
			new Thread(ramenCook,"C").start();
			new Thread(ramenCook,"D").start();
		}
		catch(Exception e)
		{
			e.printStackTrace();
		}

	}

}

interface Runnable
{
	public void run();
}


class currentThread extends Thread
{
	public RamenCook ramenCook;
	static String nam;
	
	currentThread()
	{
		this(new RamenCook(5) , "");
	}
	
	currentThread(RamenCook ramenCook , String nam)
	{
		this.ramenCook = ramenCook;
		this.nam = nam;
	}
}



class RamenCook extends Thread implements Runnable
{
	private int ramenCount;
	private String[] burners = {"_","_","_","_"};
	
	public RamenCook(int count)
	{
		ramenCount = count;
	}
	
	@Override
	public void run()
	{
		while(ramenCount > 0)
		{
			synchronized(this)
			{
				ramenCount--;
				System.out.println(Thread.currentThread().getName() + " : " + ramenCount + "개 남았습니다");
			}
			
			for(int i = 0; i < burners.length; i++)
			{
				if(!burners[i].equals("_"))
				{
					continue;
				}
				
				synchronized(this)
				{
					//if(burners[i].equals("_"))
					//{
						burners[i] = Thread.currentThread().getName();
						System.out.println("                 " + Thread.currentThread().getName() + " : [" + (i + 1) + "]번 버너 ON");
						showBurners();
					//}
				}
				
				try
				{
					Thread.sleep(2000);	
				}
				catch(Exception e)
				{
					e.printStackTrace();
				}
				
				synchronized(this)
				{
					burners[i] = "_";
					System.out.println("                                  " + Thread.currentThread().getName() + " : [" + (i + 1) + "]번 버너 OFF" );
					showBurners();
				}
				break;
			}
			
			try
			{
				Thread.sleep(Math.round(1000 * Math.random()));
			}
			catch(Exception e)
			{
				e.printStackTrace();
			}
		}
	}
	
	private void showBurners()
	{
		String stringToPrint = "                                                             ";
		for(int i = 0; i < burners.length; i++)
		{
			stringToPrint += (" " + burners[i]);
		}
		System.out.println(stringToPrint);
	}


}
```

- RamenProgram이란 클래스 안에 메인 메소드를 돌리면 RamanProgram 클래스
- RamenCook이란 별도의 클래스기 있음
-  RamencCook은 Runnable이란 인터페이스를 사용했는데 이 말은, 이 클래스에 필수적으로 run이란 함수가 있어서 스레드에서 진행할 작업을 여기다 정의하는 뜻
- 변수가 끓어야 할 라면의 수가 Interger로, 그리고 각 버너의 상태가 String 배열로 나온다
- 생성자에서 끓일 라면의 갯루를 받아와서 ramenCount에 적용
- showBurners함수를 만들어서 실행되는 시점마다 버너들의 상태를 출력
- synchronizes 블록<br>
이 안에 있는 변수들은 한 번에 한 스레드만 손을 댈 수 있음

