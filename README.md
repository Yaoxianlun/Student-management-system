# Student-management-system
Java novice,please correct it,thank you.
package work;
import java.util.*;

//添加类的接口
interface Open{
	LinkedList open(LinkedList list);
}

//指定位置添加的接口（修改）
interface openmodify{
	LinkedList open(LinkedList list,int set);
}

//添加固定的基本属
class Openset implements Open{
    public LinkedList open(LinkedList list) {
    	list.add("学号：");
    	list.add("姓名：");
    	list.add("年龄：");
		list.add("居住地：");
		return list;
	}
}

//添加的类
class Openinput implements Open{
	Scanner scanner = new Scanner(System.in);
	public LinkedList open(LinkedList list) {
		//判断输入的学号有无被占用
		String string = scanner.next();
		list.add(string);
		//判断输入的是否是学号,是则查询有无占用
		if(list.get((list.size()-5)).equals("学号：")) {
			//遍历集合
			for(int i=0;i<list.size()-6;i++) {
				//寻找集合中的学号
				if(list.get(i).equals("学号：")) {
					if(string.equals(list.get(i+4))) {
						//若输入的学号与找到的学号相同则打印学号已被占用
						if(list.size()>=8) {
							System.out.println("该学号已被占用！");
							list.removeLast();
							list.removeLast();
							list.removeLast();
							list.removeLast();
							list.removeLast();
						}
						else {
							list.removeLast();
							list.removeLast();
							list.removeLast();
							list.removeLast();
							list.removeLast();
						}
					}
				}
			}
		}
		return list;
	}
}
//修改的类
class Modify implements openmodify{
	Scanner scanner = new Scanner(System.in);
	public LinkedList open(LinkedList list,int set) {
		//添加
		list.add(set, scanner.next());
		return list;
	}
}
class Head{
	
	LinkedList list = new LinkedList();
	Openset oSet=new Openset();
	Openinput oi=new Openinput();
	Modify modify = new Modify();
	
	Scanner scanner = new Scanner(System.in);
	public Head() {
		String str1 = "";
	    itcase1:for(;!str1.endsWith("退出");) {
	    	System.out.println("-----------------------------学生管理系统首页---------------------------");
			System.out.println("查询		添加		删除		修改		退出");
			System.out.println("-----------------------------------------------------------------------");
	    	 System.out.print("请选择你要的的操作：");
	    	str1 = scanner.next();
	    	if(str1.equals("查询")) {
	    		query();
	    	}
	    	if (str1.equals("添加")) {
				addTo();
			}
	    	if (str1.equals("删除")) {
				delete();
			}
	    	if (str1.equals("修改")) {
				change();
			}
	    }
	}
	//查询
	public void query() {
		System.out.println("所有学生的信息如下：");
		for(int i=0;i<=list.size()-5;i++) {
			System.out.print(list.get(i));     //打印属类
			System.out.println(list.get(i+4)); //打印学生信息
			if(list.get(i).equals("居住地：")) {
				System.out.println();
				i=i+4;
			}
		}
	}
	//添加
	public void addTo() {
		System.out.print("是否进行添加:");
		String str2 = "";
		str2 = scanner.next();
		for(;!str2.equals("否");) {
			int j;
			oSet.open(list);                     //向集合输入属类
			j=list.size()-4;
			itcase2:for(int i=0;i<4;i++) {       //集合指向“居住地：”则退出循环
				System.out.print(list.get(j));   //打印属类
				oi.open(list);                   //输入信息
				if(list.size()!=j+5) {
					break itcase2;
				}
				j+=1;
			}
			System.out.print("是否还选择录入："); //若还进行输入则继续
			str2 = scanner.next();
		}
	}
	//删除
	public void delete() {
		System.out.print("是否进行删除：");
		String str3 = "";
		str3 = scanner.next();
		for(;!str3.equals("否");) {
			System.out.print("请输入要删除的学生的学号：");
			String str4 = scanner.next();
			//循环集合list
			for(int i=4;i<list.size()-1;) {
				//寻找与之匹配的学号
				if(str4.equals(list.get(i))) {
					//删除与该学号相关的一切信息
					for(int j=i+3;j>=i-4;j--) {
						list.remove(j);
					}
				}
				i=i+8;
			}
			System.out.print("是否还进行删除操作：");
			str3 = scanner.next();
		}
	}
	//修改
	public void change() {
		System.out.print("请输入要修改的学生的学号：");
		String str5 = scanner.next();
		//循环集合
		itcase:for(int i=4;i<list.size();) {
			//寻找与之匹配的信息
			if(list.get(i).equals(str5)) {
				//删除
				list.remove(i+3);
				list.remove(i+2);
				list.remove(i+1);
				list.remove(i);
				System.out.println("请输入要修改的信息：");
				//修改
				for(int j=i;j<=i+3;j++) {
					System.out.print(list.get(j-4));
					modify.open(list,j);
				}
				//修改完退出
				break itcase;
			}
			//若查找的没有此人信息则输出
			if(i==list.size()-3) {
				System.out.println("查无此人！");
				break itcase;
			}
			i=i+8;
		}
	}
}
public class Lianxi2 {
	
	public static void main(String[] args) {
		new Head();
	}
}
