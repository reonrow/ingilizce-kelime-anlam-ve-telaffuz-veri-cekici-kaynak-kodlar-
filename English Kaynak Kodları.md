# -ngilizce-kelime-anlam-ve-telaffuz-veri-ekici-kaynak-kodları-
C# için geçerlidir



using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Net;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.Windows.Forms;
using Microsoft.VisualBasic;
namespace English
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {

        }

        private void txtArananKelime_KeyDown(object sender, KeyEventArgs e)
        {
            if (radioButton1.Checked ==true)
            {
                if (Convert.ToInt32(e.KeyCode) == 13 && txtArananKelime.Text != "")
                {
                    try
                    {
                        var url = new Uri("https://www.okunusu.com/" + txtArananKelime.Text + "-okunusu/"); // url oluştruduk
                        var client = new WebClient(); // siteye erişim için client tanımladık
                        var html = client.DownloadString(url); //sitenin html lini indirdik
                        HtmlAgilityPack.HtmlDocument doc = new HtmlAgilityPack.HtmlDocument(); //burada HtmlAgilityPack Kütüphanesini kullandık
                        doc.LoadHtml(html); // indirdiğimiz sitenin html lini oluşturduğumuz dokumana dolduruyoruz
                        var veri1 = doc.DocumentNode.SelectNodes("/html[1]/body[1]/div[3]/div[1]/div[1]/div[1]/div[1]/p[4]/font[1]")[0];
                        var veri2 = doc.DocumentNode.SelectNodes("html[1]/body[1]/div[3]/div[1]/div[1]/div[1]/div[1]/p[2]")[0];
                        if (veri1 != null && veri2 != null)
                        {

                            txtCikanSonuc.Text = txtArananKelime.Text + "(" + veri1.InnerHtml + ")=" + Strings.Mid(veri2.InnerHtml, txtArananKelime.TextLength + 10);

                            Thread thread = new Thread(() => Clipboard.SetText(txtCikanSonuc.Text.ToString()));
                            thread.SetApartmentState(ApartmentState.STA);
                            thread.Start();
                            thread.Join();

                        }
                    }
                    catch (Exception)
                    {

                        MessageBox.Show("Arama sonucu bulunamadı veya Hatalı kelime girildi!");
                    }
                }
            }
            if (radioButton2.Checked == true)
                {
                    if (Convert.ToInt32(e.KeyCode) == 13 && txtArananKelime.Text != "")
                    {
                        try
                        {
                            var url = new Uri("https://sozluk.turkceokunusu.com/" + txtArananKelime.Text + "-okunusu/"); // url oluştruduk
                            var client = new WebClient(); // siteye erişim için client tanımladık
                            var html = client.DownloadString(url); //sitenin html lini indirdik
                            HtmlAgilityPack.HtmlDocument doc = new HtmlAgilityPack.HtmlDocument(); //burada HtmlAgilityPack Kütüphanesini kullandık
                            doc.LoadHtml(html); // indirdiğimiz sitenin html lini oluşturduğumuz dokumana dolduruyoruz
                            var veri1 = doc.DocumentNode.SelectNodes("/html[1]/body[1]/div[1]/main[1]/div[1]/div[1]/article[1]/div[2]/div[2]/div[1]/strong[2]/h2[1]")[0];
                            var veri2 = doc.DocumentNode.SelectNodes("/html[1]/body[1]/div[1]/main[1]/div[1]/div[1]/article[1]/div[2]/div[4]/div[1]/h2[1]")[0];
                            if (veri1 != null && veri2 != null)
                            {

                            string gecicihafiza;
                            gecicihafiza = txtArananKelime.Text + "(" + veri1.InnerHtml + ")=" + veri2.InnerHtml;
                            txtCikanSonuc.Text = Strings.Mid(gecicihafiza, 1,gecicihafiza.Length-9);   

                            Thread thread = new Thread(() => Clipboard.SetText(txtCikanSonuc.Text.ToString()));
                                thread.SetApartmentState(ApartmentState.STA);
                                thread.Start();
                                thread.Join();

                            }
                        }
                        catch (Exception)
                        {

                            MessageBox.Show("Arama sonucu bulunamadı veya Hatalı kelime girildi!");
                        }
                    }
                }
        }
    }
}

