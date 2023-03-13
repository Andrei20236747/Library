# Library
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Data.SqlClient;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Библиотека_техникума
{
    public partial class avtoriz : Form
    {
        int K = 0;
        DateTime ed;
        public avtoriz()
        {
            InitializeComponent();
        }

        private void woiti_Click(object sender, EventArgs e)
        {
            if ((parol.Text != "111" && parol.Enabled) || (parol.Text != "222" && parol.Enabled))
            {
                K++;
                if (K == 3)
                {
                    ed = DateTime.Now.AddSeconds((K + 1) * 10);

                    parol.Enabled = false;
                    timer1.Start();
                }

                SqlConnection con = new SqlConnection("Data Source=WIN-BAL03D5CQS3;Initial Catalog=Техникум;Integrated Security=True");
                SqlCommand cmd = new SqlCommand("SELECT*from Пользователи where Id='" + Login.Text + " 'and Пароль ='" + parol.Text + "'", con);
                SqlDataAdapter sda = new SqlDataAdapter(cmd);
                DataTable dt = new DataTable();
                sda.Fill(dt);
                string cmbItemValue = rol.SelectedItem.ToString();
                for (int i = 0; i < dt.Rows.Count; i++)
                {
                    if (dt.Rows[i]["Роль"].ToString() == cmbItemValue)
                    {
                        if (rol.SelectedIndex == 0)
                        {
                            MessageBox.Show("Вы успешно вошли в систему"); K = 0;
                            menu ff = new menu(dt.Rows[0][1].ToString());
                            ff.Show();
                            this.Hide();
                        }

                        else if (rol.SelectedIndex == 1)
                        {
                            MessageBox.Show("Вы успешно вошли в систему"); K = 0;
                            infopolz gg = new infopolz(dt.Rows[0][1].ToString());
                            gg.Show();
                            this.Hide();
                        }
                    }
                    
                }
            }
            else MessageBox.Show("Вход заблокирован!");
        }

        private void button1_Click(object sender, EventArgs e)
        {
            registr form2 = new registr();
            form2.Show();
            this.Hide();
        }

        private void avtoriz_Load(object sender, EventArgs e)
        {
            parol.Enabled = false;
            rol.Enabled = false;
            woiti.Enabled = false;
        }

        private void Login_KeyDown(object sender, KeyEventArgs e)
        {
            if (e.KeyCode == Keys.Enter)
            {
                SqlConnection connection = new SqlConnection(@"Data Source=WIN-BAL03D5CQS3;Initial Catalog=Техникум;Integrated Security=True");
                SqlDataAdapter adapter = new SqlDataAdapter("Select * FROM Пользователи where Id ='" + Login.Text + "'", connection);
                DataTable dt = new DataTable();
                adapter.Fill(dt);
                if (dt.Rows.Count == 1)
                {

                    parol.Enabled = true;
                    rol.Enabled = true;
                    woiti.Enabled = true;
                    parol.Focus();
                }
                else
                    MessageBox.Show("Id отсутствует в БД");
            }
        }

        private void parol_KeyDown(object sender, KeyEventArgs e)
        {
            if (e.KeyCode == Keys.Enter)
            {
                SqlConnection connection = new SqlConnection(@"Data Source=WIN-BAL03D5CQS3;Initial Catalog=Техникум;Integrated Security=True");
                SqlDataAdapter adapter = new SqlDataAdapter("Select * FROM Пользователи where Пароль ='" + parol.Text + "'", connection);
                DataTable dt = new DataTable();
                adapter.Fill(dt);
                if (dt.Rows.Count == 1)
                {
                    Random rnd = new Random();
                    Text = String.Empty;
                    string ALF = "1234";
                    for (int i = 0; i < 4; ++i)
                        Text += ALF[rnd.Next(ALF.Length)];
                    MessageBox.Show(Text);


                }
                else
                    MessageBox.Show("Пароль введен неверно");
            }
        }

        private void pictureBox2_Click(object sender, EventArgs e)
        {
            Random rnd = new Random();
            Text = String.Empty;
            string ALF = "47853";
            for (int i = 0; i < 4; ++i)
                Text += ALF[rnd.Next(ALF.Length)];
            MessageBox.Show(Text);
        }

        private void timer1_Tick(object sender, EventArgs e)
        {
            if (DateTime.Now > ed) { parol.Enabled = true; parol.Enabled = true; timer1.Stop(); }
        }

        private void pictureBox1_Click(object sender, EventArgs e)
        {

        }
    }
}
