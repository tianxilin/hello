# hello
111111：
package book;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.*;
import java.text.*;
public class TransactCard implements ActionListener
{
private  JButton btn1,btn2,btn3,btn4;
private  JTextField tfd1,tfd3,tfd4,tfd5;
private  JComboBox jcb1,jcb2,jcb3;
private  PreparedStatement pstmt;//SQL	语句对象
private  Connection con;//数据库连接对象
private  JFrame frame;
private static Statement stmt;
public TransactCard()
{
Font s=new Font("楷体",Font.PLAIN,12);
UIManager.put("Component.font",s);
UIManager.put("Label.font",s);
UIManager.put("ComboBox.font",s);
UIManager.put("Button.font",s);
UIManager.put("Menu.font",s);
UIManager.put("MenuItem.font",s);
frame=new JFrame("图书证办理");
Container content=frame.getContentPane();
frame.addWindowListener(new WindowAdapter()//frame添加窗口事件监听给窗口适配器
{
@SuppressWarnings("unused")
public void windowCloing(WindowEvent e)//利用窗口关闭框关闭窗口
{
try{
pstmt.close();
con.close();
stmt.close();
frame.dispose();
}
catch(SQLException sqle)
{
}
}
});
content.setLayout(new BorderLayout());
JPanel Jpl=new JPanel();
JPanel Jpl2=new JPanel();
Jpl.setLayout(new GridLayout(7,2,2,6));
Jpl2.setLayout(new GridLayout(1,4,10,0));
JLabel lb1=new JLabel("  姓名:");
JLabel lb2=new JLabel("  性别:");
JLabel lb3=new JLabel("  身份:");
JLabel lb4=new JLabel("  学院:");
JLabel lb5=new JLabel("  证件号码:");
JLabel lb6=new JLabel("  注册日期:");
JLabel lb7=new JLabel("  有效日期:");
btn1=new JButton("添加");
btn2=new JButton("删除");
btn3=new JButton("撤消");
btn4=new JButton("退出");
btn1.addActionListener(this);//注册动作事件监听
btn2.addActionListener(this);
btn3.addActionListener(this);
btn4.addActionListener(this);
tfd1=new JTextField();
tfd3=new JTextField();
tfd4=new JTextField();
tfd5=new JTextField();
tfd4.addFocusListener(new FocusHandler());//内部类处理焦点事件监听
tfd5.addFocusListener(new FocusHandler());
String[] str_sex={"男","女"};
String[] str_office={"计算机系","电子系","商学系","机电系","外语系"};
String[] str_status={"学生","教师"};
jcb1=new JComboBox(str_sex);
jcb2=new JComboBox(str_status);
jcb3=new JComboBox(str_office);
jcb2.setEditable(true);
Jpl.add(lb1);//姓名
Jpl.add(tfd1);
Jpl.add(lb2);//性别
Jpl.add(jcb1);
Jpl.add(lb3);//身份
Jpl.add(jcb2);
Jpl.add(lb4);//学院
Jpl.add(jcb3);
Jpl.add(lb5);//图书证号
Jpl.add(tfd3);
Jpl.add(lb6);//注册日期
Jpl.add(tfd4);
Jpl.add(lb7);//有效日期
Jpl.add(tfd5);
Jpl.setBorder(BorderFactory.createTitledBorder("图书证注册"));
Jpl2.add(btn1);
Jpl2.add(btn2);
Jpl2.add(btn3);
Jpl2.add(btn4);
Jpl2.setBorder(BorderFactory.createLineBorder(Color.DARK_GRAY));
content.add(Jpl,BorderLayout.CENTER);
content.add(Jpl2,BorderLayout.SOUTH);
frame.pack();
frame.setLocation(150,100);
frame.setSize(450,400);
frame.setVisible(true);
try{
Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
con=DriverManager.getConnection("jdbc:odbc:library");
String str;
str="insert into user (user_name,user_sex,user_status,user_office,"+
"user_cardnumber,user_registerdate,user_canceldate,user_state) values(?,?,?,?,?,?,?,'正常')";
pstmt=con.prepareStatement(str,ResultSet.TYPE_SCROLL_INSENSITIVE,
ResultSet.CONCUR_UPDATABLE);
stmt=con.createStatement();
}
catch(SQLException e)
{
}
catch(ClassNotFoundException cnfe)
{
}
}
class FocusHandler implements FocusListener
{
java.util.Date today=new java.util.Date();
java.util.Date today2=new java.util.Date();
String formatted2;
DateFormat fmt=DateFormat.getDateInstance();
String formatted=fmt.format(today);

@SuppressWarnings("deprecation")
public void focusGained(FocusEvent e)
{
Object obj=(JTextField)e.getSource();
if(obj==tfd4)
{
tfd4.setText(formatted);
}
if(obj==tfd5)
{
today2.setYear(today.getYear()+4);
formatted2=fmt.format(today2);
tfd5.setText(formatted2);
}
}
public void focusLost(FocusEvent e)
{
}
}
public void actionPerformed(ActionEvent e)
{
String str1,str2,str3,str4,str5,str6,str7;
java.sql.Date today1,today2;
str1=tfd1.getText().trim();//姓名
str2=jcb1.getSelectedItem().toString();//性别
str3=jcb2.getSelectedItem().toString();//性别
str4=jcb3.getSelectedItem().toString();//系别
str5=tfd3.getText().trim();//学号
str6=tfd4.getText().trim();//注册日期
str7=tfd5.getText().trim();//有效日期
System.out.println("act");
String sqlStr="delete from user where user_cardnumber="+"'"+str5+"'";
Object obj=(JButton)e.getSource();
try{
if(obj==btn1)
{
JOptionPane.showMessageDialog(frame,"注册成功！");
today1=java.sql.Date.valueOf(str6);
today2=java.sql.Date.valueOf(str7);
pstmt.setString(1,str1);
pstmt.setString(2,str2);
pstmt.setString(3,str3);
pstmt.setString(4,str4);
pstmt.setString(5,str5);
pstmt.setDate(6,today1);
pstmt.setDate(7,today2);
pstmt.executeUpdate();
}
else if(obj==btn2)
{
stmt.executeUpdate(sqlStr);
}
else if(obj==btn3)
{
tfd1.setText("");
tfd3.setText("");
tfd4.setText("");
tfd5.setText("");
}
else if(obj==btn4)
{
pstmt.close();
stmt.close();
con.close();
frame.dispose();
}
}
catch(SQLException sqle)
{
System.err.println(sqle.getMessage());
}
}
public static void main(String[]args)
{
new TransactCard();
}
}
22222222222：
package book;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.*;
public class Login extends JFrame
{
private static final long serialVersionUID = 1L;
private  JLabel JLb1;
private  JLabel JLb2;
private  JButton Ok_btn;
private  JButton Cancel_btn;
private  JTextField Jtfld1;
private  JPasswordField Jtfld2;
private JFrame frame;
private Connection con;
private Statement stmt;
public Login()
{
JFrame.setDefaultLookAndFeelDecorated(true);
frame=new JFrame("登陆");
frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
Container content=frame.getContentPane();
content.setLayout(new GridLayout(3,2,20,20));
JLb1=new JLabel("用户名:");
JLb2=new JLabel("密码:");
Jtfld1=new JTextField();
Jtfld2=new JPasswordField();
Ok_btn=new JButton("确定");
Cancel_btn=new JButton("取消");
Ok_btn.addActionListener(new ActionHandler());
Cancel_btn.addActionListener(new ActionHandler());
content.add(JLb1);
content.add(Jtfld1);
content.add(JLb2);
content.add(Jtfld2);
content.add(Ok_btn);
content.add(Cancel_btn);
frame.pack();
frame.setLocationRelativeTo(null);
frame.setSize(300,200);
frame.setVisible(true);

try{
Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
con=DriverManager.getConnection("jdbc:odbc:library");
stmt=con.createStatement();
}
catch(ClassNotFoundException e)
{
}
catch(SQLException ex)
{
}
}
class ActionHandler implements ActionListener
{
@SuppressWarnings("deprecation")
public void actionPerformed(ActionEvent e)
{
String str1,str2,username,sqlStr;
Object obj=e.getSource();
str1=Jtfld1.getText().trim();
str2=Jtfld2.getText().trim();
try{
if(obj.equals(Ok_btn))
{
if(str1.equals(""))
{
JOptionPane.showMessageDialog(frame,"用户名不能为空");
return;
}
sqlStr="select * from login where user_name="+"'"+str1+"'"+
" and psw="+"'"+str2+"'";
ResultSet result=stmt.executeQuery(sqlStr);
if(result.next())
{
username=result.getString("user_name");
if(str1.equals("管理员"))
{
JOptionPane.showMessageDialog(frame,"欢迎光临！");
new MainFrame();
}
else
{
JOptionPane.showMessageDialog(frame,"欢迎光临！");
MainFrame mainFrame=new MainFrame();
mainFrame.menuItem3.setEnabled(false);
mainFrame.menuItem6.setEnabled(false);
mainFrame.menuItem7.setEnabled(false);
mainFrame.menuItem8.setEnabled(false);
mainFrame.menuItem9.setEnabled(false);
}
frame.dispose();
stmt.close();
con.close();
}
else
{
JOptionPane.showMessageDialog(frame,"输入名或密码错误！");
}
}
else
{
frame.dispose();
stmt.close();
con.close();
}
}
catch(SQLException ex)
{
System.err.println(ex);
}
}
}
public static void main(String[]args)
{
javax.swing.SwingUtilities.invokeLater(new Runnable()
{
public void run()
{
new Login();
}
});
}
}
3333333：
package book;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import java.sql.*;
import java.text.*;
import java.math.*;
public class Book implements ActionListener
{
private  JButton btn1,btn2,btn3,btn4;
private JTextField jtfd1,jtfd2,jtfd3,jtfd5,jtfd6,
jtfd7,jtfd8,jtfd9,jtfd10;
private JComboBox jcbx;
private  JTextArea jta;
private  Connection con;
private  PreparedStatement pstmt1,pstmt2;
private  JFrame frame;
@SuppressWarnings("static-access")
public Book()
{
Font s=new Font("楷体",Font.PLAIN,12);
UIManager.put("Component.font",s);
UIManager.put("Label.font",s);
UIManager.put("ComboBox.font",s);
UIManager.put("Button.font",s);
UIManager.put("Menu.font",s);
UIManager.put("MenuItem.font",s);
frame=new JFrame("图书入库");
Container content=frame.getContentPane();
Toolkit tool=frame.getToolkit();
@SuppressWarnings("unused")
Dimension wndsize=tool.getScreenSize();
JLabel lb1=new JLabel("  书名:");
JLabel lb2=new JLabel("  条形码:");
JLabel lb3=new JLabel("  分类号:");
JLabel lb4=new JLabel("  分类名:");
JLabel lb5=new JLabel("  排架号:");
JLabel lb6=new JLabel("  出版社:");
JLabel lb7=new JLabel("  出版日期:");
JLabel lb8=new JLabel("  入库日期:");
JLabel lb9=new JLabel("  价格:");
JLabel lb10=new JLabel("  作者:");
JLabel lb11=new JLabel("  简介:");
jtfd1=new JTextField();
jtfd2=new JTextField();
jtfd3=new JTextField();
String[] kindname={"马克思列宁主义、毛泽东思想","综合性图书","哲学","社会科学总论",
"政治、法律","军事","经济","文化、科学、教育、体育","语言文字",
"文学","天文学、地球科学","生物科学","医药、卫生","农业科学",
"工业技术","艺术","历史、地理","数理化","自然科学总论",
"环境科学","航空航天","交通"};
jcbx=new JComboBox(kindname);//组合按钮，类似下拉列表
jtfd5=new JTextField();
jtfd6=new JTextField();
jtfd7=new JTextField();
jtfd8=new JTextField();
jtfd9=new JTextField();
jtfd10=new JTextField();
jtfd8.addFocusListener(new FocusHandler());//内部类处理焦点事件
jta=new JTextArea();//文本域
jta.setLineWrap(true);//分配文本域
btn1=new JButton("添加");
btn2=new JButton("删除");
btn3=new JButton("撤消");
btn4=new JButton("退出");
btn1.addActionListener(this);
btn2.addActionListener(this);
btn3.addActionListener(this);
btn4.addActionListener(this);
JPanel pl1=new JPanel();
JPanel pl2=new JPanel();
JPanel pl3=new JPanel();
JPanel pl4=new JPanel();
pl1.setLayout(new GridLayout(5,4,6,6));
pl1.add(lb1);//书名
pl1.add(jtfd1);
pl1.add(lb2);//条形码
pl1.add(jtfd2);
pl1.add(lb10);//作者
pl1.add(jtfd10);
pl1.add(lb3);//分类号
pl1.add(jtfd3);
pl1.add(lb4);//分类名
pl1.add(jcbx);
pl1.add(lb5);//排架号
pl1.add(jtfd5);
pl1.add(lb6);//出版社
pl1.add(jtfd6);
pl1.add(lb7);//出版日期
pl1.add(jtfd7);
pl1.add(lb8);//入库日期
pl1.add(jtfd8);
pl1.add(lb9);//价格
pl1.add(jtfd9);
GridBagLayout gridbag=new GridBagLayout();
GridBagConstraints constraints=new GridBagConstraints();
pl2.setLayout(gridbag);
constraints.weightx=constraints.weighty=10.0;
constraints.fill=constraints.BOTH;
gridbag.setConstraints(lb11,constraints);
constraints.weightx=1;
constraints.gridwidth=1;
pl2.add(lb11,constraints);
gridbag.setConstraints(jta,constraints);
constraints.weightx=9;
constraints.gridheight=3;
constraints.gridwidth=3;
constraints.insets=new Insets(10,15,10,20);
pl2.add(jta,constraints);
GridBagLayout gridbag1=new GridBagLayout();
GridBagConstraints constraints1=new GridBagConstraints();
constraints1.weightx=1.0;
constraints1.fill=constraints1.BOTH;
pl3.setLayout(gridbag1);
constraints1.weighty=0.8;
constraints1.gridwidth=constraints1.REMAINDER;
pl3.add(pl1,constraints1);
constraints1.weighty=0.2;
constraints1.gridheight=constraints.REMAINDER;
pl3.add(pl2,constraints1);
pl4.setLayout(new GridLayout(1,4,6,0));
pl4.add(btn1);
pl4.add(btn2);
pl4.add(btn3);
pl4.add(btn4);
content.setLayout(new BorderLayout());
content.add(pl3,BorderLayout.CENTER);
content.add(pl4,BorderLayout.SOUTH);
frame.setBounds(100,100,550,400);
frame.setResizable(false);
frame.setVisible(true);
try
{
String sqlStr1,sqlStr2;
sqlStr1="insert into book (bookname,bannercode,kindnumber,kindname,"+
"positionnumber,publishingcompany,publishtime,putintime,price,"+
"state,introduction,author) values(?,?,?,?,?,?,?,?,?,'在架',?,?)";
sqlStr2="delete from book where bannercode=?";
Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
con=DriverManager.getConnection("jdbc:odbc:library");
pstmt1=con.prepareStatement(sqlStr1);
pstmt2=con.prepareStatement(sqlStr2);
}
catch(ClassNotFoundException e)
{
}
catch(SQLException sqle)
{
}
}
class FocusHandler implements FocusListener
{
java.util.Date today=new java.util.Date();
DateFormat format=DateFormat.getDateInstance();
String formatted=format.format(today);
public void focusGained(FocusEvent e)
{
Object obj=(JTextField)e.getSource();
if(obj==jtfd8)
{
jtfd8.setText(formatted);
}
}
public void focusLost(FocusEvent e)
{
}
}
public void actionPerformed(ActionEvent e)
{
String str1,str2,str3,str4,str5,str6,
str7,str8,str9,str10,str11;
str1=jtfd1.getText().trim();//书名
str2=jtfd2.getText().trim();//条形码
str3=jtfd3.getText().trim();//分类号
str4=(String)jcbx.getSelectedItem();//分类名
str5=jtfd5.getText().trim();//排架号
str6=jtfd6.getText().trim();//出版社
str7=jtfd7.getText().trim();//出版日期
str8=jtfd8.getText().trim();//入库日期
str9=jtfd9.getText().trim();//价格
str10=jtfd10.getText().trim();//作者
str11=jta.getText().trim();//文本域
int result;
try{
Object obj=(JButton)e.getSource();
if(obj==btn1)
{
if(str1.equals("")|str2.equals("")|str3.equals("")|str4.equals("")|str5.equals(""))
{
JOptionPane.showMessageDialog(frame,"不能为空!");
return;
}
if(str6.equals("")|str7.equals("")|str8.equals("")|str9.equals("")|str10.equals(""))
{
JOptionPane.showMessageDialog(frame,"不能为空!");
return;
}
java.sql.Date today1,today2;
BigDecimal money=new BigDecimal(str9);
today1=java.sql.Date.valueOf(str7);
today2=java.sql.Date.valueOf(str8);
pstmt1.setString(1,str1);
pstmt1.setString(2,str2);
pstmt1.setString(3,str10);
pstmt1.setString(4,str4);
pstmt1.setString(5,str5);
pstmt1.setString(6,str6);
pstmt1.setDate(7,today1);
pstmt1.setDate(8,today2);
pstmt1.setBigDecimal(9,money);
pstmt1.setString(10,str11);
pstmt1.setString(11,str10);
result=pstmt1.executeUpdate();
if(result>0)
{
JOptionPane.showMessageDialog(frame,"图书添加成功!");
}
jtfd1.setText("");
jtfd2.setText("");
jtfd3.setText("");
jtfd5.setText("");
jtfd6.setText("");
jtfd7.setText("");
jtfd8.setText("");
jtfd9.setText("");
jtfd10.setText("");
jta.setText("");
}
else if(obj==btn2)
{
if(str2.equals(""))//条形码
{
JOptionPane.showMessageDialog(frame,"删出图书不能为空!");
return;
}
pstmt2.setString(1,str2);
result=pstmt2.executeUpdate();
if(result>0)
{
JOptionPane.showMessageDialog(frame,"图书删除成功!");
}
}
else if(obj==btn3)
{
jtfd1.setText("");
jtfd2.setText("");
jtfd3.setText("");
jtfd5.setText("");
jtfd6.setText("");
jtfd7.setText("");
jtfd8.setText("");
jtfd9.setText("");
jtfd10.setText("");
jta.setText("");
}
else if(obj==btn4)
{
pstmt1.close();
pstmt2.close();
con.close();
frame.dispose();
}
}
catch(SQLException sqle)
{
System.err.println(sqle);
}
}
public static void main(String[]args)
{
new Book();
}
}
4444444：
package book;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.*;
public class BookInfo implements ActionListener
{
private JFrame frame;
private Statement stmt;
private Connection con;
private JTable tableTest;
private String[]columnNames={"书名","条形码","分类号","分类名","排架号","出版社",
"出版日期","入库日期","状态","简介"};
private Object[][]rowData=new Object[100][10];
private JButton btn;
private JRadioButton rbtn1,rbtn2;
private JTextField jtfd;
public BookInfo()
{
Font s=new Font("楷体",Font.PLAIN,12);
UIManager.put("Component.font",s);
UIManager.put("Label.font",s);
UIManager.put("ComboBox.font",s);
UIManager.put("Button.font",s);
UIManager.put("Menu.font",s);
UIManager.put("MenuItem.font",s);
frame=new JFrame("图书信息查询");
Container content=frame.getContentPane();
btn=new JButton("查询");
jtfd=new JTextField();
JPanel pl=new JPanel();
rbtn1=new JRadioButton("书名");
rbtn2=new JRadioButton("条形码");
rbtn1.setSelected(true);
btn.addActionListener(this);
ButtonGroup group=new ButtonGroup();
group.add(rbtn1);
group.add(rbtn2);
pl.setLayout(new GridLayout(1,4,10,0));
pl.add(rbtn1);
pl.add(rbtn2);
pl.add(jtfd);
pl.add(btn);
tableTest=new JTable(rowData,columnNames);
tableTest.setRowHeight(20);
tableTest.setPreferredScrollableViewportSize(new Dimension(500, 30));
JScrollPane scrollPane=new JScrollPane(tableTest);
content.add(pl,BorderLayout.NORTH);
content.add(scrollPane,BorderLayout.CENTER);
frame.pack();
frame.setLocation(100,150);
frame.setSize(850,300);
frame.setVisible(true);
try
{
Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
con=DriverManager.getConnection("jdbc:odbc:library");
stmt=con.createStatement();
}
catch(ClassNotFoundException e)
{
System.err.println(e.getMessage());
}
catch(SQLException e)
{
System.err.println(e.getMessage());
}
}
public void actionPerformed(ActionEvent e)
{
Object obj=e.getSource();
ResultSet result;
String sqlStr;
int i=0;
String str=jtfd.getText().trim();
if(rbtn2.isSelected())
{
sqlStr="select bookname,bannercode,kindnumber,kindname,positionnumber,"+
"publishingcompany,publishtime,putintime,state,introduction from book where "+
"bannercode="+"'"+str+"'";
}
else
{
sqlStr="select bookname,bannercode,kindnumber,kindname,positionnumber,"+
"publishingcompany,publishtime,putintime,state,introduction from book where "+
"bookname like '"+str+"%'";
}
try
{
if(obj==btn)
{
if(str.equals(""))
{
JOptionPane.showMessageDialog(frame,"查询输入不能为空!");
return;
}
result=stmt.executeQuery(sqlStr);
for(int j=0;j<rowData.length;j++)
{
for(int k=0;k<10;k++)
rowData[j][k]=null;
}
tableTest.repaint();
while(result.next())
{
if(i<rowData.length)
{
rowData[i][0]=result.getString("bookname");
rowData[i][1]=result.getString("bannercode");
rowData[i][2]=result.getString("kindnumber");
rowData[i][3]=result.getString("kindname");
rowData[i][4]=result.getString("positionnumber");
rowData[i][5]=result.getString("publishingcompany");
rowData[i][6]=result.getDate("publishtime");
rowData[i][7]=result.getDate("putintime");
rowData[i][8]=result.getString("state");
rowData[i][9]=result.getString("introduction");
i++;
}
else
{
JOptionPane.showMessageDialog(frame,"please specify the bookname!");
}
}
jtfd.setText("");
}
}
catch(SQLException sqle)
{
System.err.println(sqle);
}
}
public static void main(String[]args)
{
new BookInfo();
}
}
