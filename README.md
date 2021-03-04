using HtmlAgilityPack;
using System;
using System.IO;
using System.Linq;
using System.Net.Http;

namespace ConsoleApp1POGya
{
    class Program
    {
        private const string UrlTemplate = @"https://yandex.ru/pogoda/kotlas";
        public static async void GetWeather()
        {
           
            DateTime time = DateTime.Now;
            int second = time.Second;
            int minute = time.Minute;
            string stroka1 = "";
            string stroka2 = "";
            string stroka3 = "";
            string stroka4 = "";
            string stroka5 = "";
            if (second == 20)
            {
                string[] arr = File.ReadLines(path: "C:\\Program Files (x86)\\InSAT\\MasterSCADA\\gw.txt").ToArray();//считываем данные
                stroka1 = arr[0];
                stroka2 = arr[1];
                stroka3 = arr[2];
                stroka4 = arr[3];
                stroka5 = arr[4];
            }
            var requestMessage = new HttpRequestMessage(HttpMethod.Get, UrlTemplate);
            HttpClient httpClient = new HttpClient();
            HttpResponseMessage responce = await httpClient.SendAsync(requestMessage);
            var res = await responce.Content.ReadAsStringAsync();
            HtmlDocument doc = new HtmlDocument();
            doc.LoadHtml(res);
            HtmlNode span = doc.DocumentNode.SelectSingleNode("//div[@class='temp fact__temp fact__temp_size_s']");
            Console.WriteLine($"надпись1 - {span.InnerText}");
           // File.WriteAllText("gw4.txt", $"{stroka1}"+"\n324\n341\n3412\n34512\n" + $"{span.InnerText}");// создаем файл
            //File.WriteAllText("gw4.txt", "3412\n324\n341\n3412\n34512\n"+ $"{span.InnerText}");// создаем файл
            //File.WriteAllText("gw4.txt", $"{span.InnerText}");// создаем файл
            if (second == 20)
            {
                File.WriteAllText("gw4.txt", $"{stroka1}" + "\n324\n341\n3412\n34512\n" + $"{span.InnerText}");// создаем файл
            }
        }
        static void Main(string[] args)
        {
           GetWeather();
           Console.Read();
        }

    }

}
