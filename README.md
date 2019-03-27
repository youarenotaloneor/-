using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Calculator
{
    public partial class 计算器 : Form
    {
        public static List<char> inputStr = new List<char>(1000);
        public 计算器()
        {
            InitializeComponent();
        }
        public void addaNum(int num)
        {
            textBox1.Text = textBox1.Text + num.ToString();
        }

        private void button11_Click(object sender, EventArgs e)
        {
            inputStr.Add('*');

            textBox1.AppendText("*");
        }

        private void button8_Click(object sender, EventArgs e)
        {
            inputStr.Add('7');

            textBox1.AppendText("7");
        }

        private void button12_Click(object sender, EventArgs e)
        {
            inputStr.Add('/');

            textBox1.AppendText("/");
        }

        private void button1_Click(object sender, EventArgs e)
        {
            inputStr.Add('8');

            textBox1.AppendText("8");
        }

        private void textBox1_TextChanged(object sender, EventArgs e)
        {
            
        }

        private void button7_Click(object sender, EventArgs e)
        {
            textBox1.Text = "";

            inputStr.Clear();
            textBox2.Text = "";

            inputStr.Clear();
        }

        private void button6_Click(object sender, EventArgs e)
        {
            inputStr.Add('9');

            textBox1.AppendText("9");
        }

        private void button14_Click(object sender, EventArgs e)
        {
            inputStr.Add('4');

            textBox1.AppendText("4");
        }

        private void button13_Click(object sender, EventArgs e)
        {
            inputStr.Add('5');

            textBox1.AppendText("5");
        }

        private void button2_Click(object sender, EventArgs e)
        {
            inputStr.Add('6');

            textBox1.AppendText("6");
        }

        private void button15_Click(object sender, EventArgs e)
        {
            inputStr.Add('1');

            textBox1.AppendText("1");
        }

        private void button10_Click(object sender, EventArgs e)
        {
            inputStr.Add('2');

            textBox1.AppendText("2");
        }

        private void button5_Click(object sender, EventArgs e)
        {
            inputStr.Add('3');

            textBox1.AppendText("3");
        }

        private void button16_Click(object sender, EventArgs e)
        {
            textBox1.AppendText("=");

            textBox1.Text = textBox1.Text;

           textBox2.Text = DataOp.DataMain();
           string temp = DataOp.DataMain();
            inputStr.Clear();
            for (int i = 0; i < temp.Length; i++)
            {
                inputStr.Add(temp[i]);
            }
          
        }

        private void button17_Click(object sender, EventArgs e)
        {
            
        }

        private void button18_Click(object sender, EventArgs e)
        {
            inputStr.Add('0');
            textBox1.AppendText("0");
        }

        private void button19_Click(object sender, EventArgs e)
        {
            inputStr.Add('.');

            textBox1.AppendText(".");
        }

        private void button17_Click_1(object sender, EventArgs e)
        {
            inputStr.Add('(');

            textBox1.AppendText("(");
        }

        private void button20_Click(object sender, EventArgs e)
        {
            inputStr.Add(')');

            textBox1.AppendText(")");
        }

        private void button9_Click(object sender, EventArgs e)
        {
            inputStr.Add('+');

            textBox1.AppendText("+");
        }

        private void button4_Click(object sender, EventArgs e)
        {
            inputStr.Add('-');

            textBox1.AppendText("-");
        }

        private void button3_Click(object sender, EventArgs e)
        {

            //界面撤销
            try { inputStr.RemoveAt(inputStr.Count - 1); }
            catch(System.ArgumentOutOfRangeException)
            {
                textBox1.Text = ("输入错误");
            }
            

            textBox1.Text = "";
            for (int i = 0; i < inputStr.Count; i++)
            {
                textBox1.Text += inputStr[i];
            }
            
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }

        private void textBox2_TextChanged(object sender, EventArgs e)
        {

        }
    }
    class DataOp : 计算器
    {
        //表达式存储在inputStr
        static Stack<double> m = new Stack<double>();//数字栈       
        static Stack<char> s = new Stack<char>();//符号栈        
        public static void Read()   //Read()从inputStr输入流中读值       
        {
            for (int i = 0; i < inputStr.Count; i++)
            {      if (!IsOperator(inputStr[i]))   //数字和小数点               
                    {
                    string s = null;
                    while (i < inputStr.Count && !IsOperator(inputStr[i]))
                    {   s += inputStr[i];
                        i++;
                    }
                    i--;
                    double mm = Convert.ToDouble(s);
                    m.Push(mm);
                }
                else if (IsOper(inputStr[i]))   //+ - * /                
                {
                    if (s.Count.Equals(0) || s.Peek().Equals('('))
                    {
                        s.Push(inputStr[i]);
                    }
                    else if (OperatorPrecedence(inputStr[i]) > OperatorPrecedence(s.Peek()))
                    {
                        s.Push(inputStr[i]);
                    }
                    else
                    {
                        double n1, n2;
                        char s1;
                        
                        try {n2 = m.Pop();n1 = m.Pop();s1 = s.Pop();
                        double sum = Operat(n1, n2, s1);
                        m.Push(sum);
                        s.Push(inputStr[i]); }
                        catch(System.InvalidOperationException)
                        {
                            MessageBox.Show("Error");


                        }
                        
                        
                    }
                }
                else                    //（和）                
                {
                    if (inputStr[i].Equals('('))
                    {
                        s.Push(inputStr[i]);
                    }
                    else if (inputStr[i].Equals(')'))
                    {
                        while (!s.Peek().Equals('('))
                        {
                            double n1, n2;
                            char s1;
                            n2 = m.Pop();
                            n1 = m.Pop();
                            s1 = s.Pop();
                            double sum = Operat(n1, n2, s1);
                            m.Push(sum);
                        }
                        s.Pop();
                    }
                }
            }
        }
        public static double PopStack()
        {
            double sum = 0;
            while (s.Count != 0)
            {
                double n1, n2;
                char s1;
                
                try
                {
                    n2 = m.Pop();
                    n1 = m.Pop();
                    s1 = s.Pop();
                    sum = Operat(n1, n2, s1);
                }

                 catch (System.InvalidOperationException)
               {
                    MessageBox.Show("Error");
               }
                
              
                /*n2 = m.Pop();
                n1 = m.Pop();
                s1 = s.Pop();
                sum = Operat(n1, n2, s1);
                m.Push(sum);*/
            }
            return sum;
        }
        public static bool IsOperator(char c)   //是否是操作符        
        {
            if (c.Equals('+') || c.Equals('-') || c.Equals('*') || c.Equals('/') || c.Equals('(') || c.Equals(')'))
                return true;
            return false;
        }
        public static bool IsOper(char c)   //是否是运算符       
        {
            if (c.Equals('+') || c.Equals('-') || c.Equals('*') || c.Equals('/'))
                return true;
            return false;
        }
        public static int OperatorPrecedence(char a)    //操作符优先级        
        {
            int i = 0;
            switch (a)
            {
                case '+': i = 3; break;
                case '-': i = 3; break;
                case '*': i = 4; break;
                case '/': i = 4; break;            }
            return i;
        }
        public static double Operat(double n1, double n2, char s1)
        {
            double sum = 0;
            switch (s1)
            {
                case '+': sum = n1 + n2; break;
                case '-': sum = n1 - n2; break;
                case '*': sum = n1 * n2; break;
                case '/': if (n2 == 0) { MessageBox.Show("Error"); } sum = n1 / n2; break;
            }
            return sum;
        }
        public static string DataMain()
        {            Read();
            return PopStack().ToString();
        }
    }


}

