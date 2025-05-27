import org.jsoup.Jsoup;
import java.util.Scanner;
import java.util.HashMap;
import java.util.Map;
class Currency {
  private String name; //объявляем переменные
  private double priceVsNis;

  public Currency(String name, double priceVsNis) { //создаем конструктор
    this.priceVsNis = priceVsNis;
  }

  public static Map<String, Currency> createMap(){ //создаем map и внутри него создаем объекты на основе конструктора Currency
  Map<String, Currency> currencies = new HashMap<>();
  try{
    currencies.put("ZAR", new Currency("Южноизраильский Рэнд", Double.parseDouble(Jsoup.connect("https://ru.investing.com/currencies/usd-ils").get().selectFirst("div[data-test=instrument-price-last]").text().replace(",","."))));
    currencies.put("IRR", new Currency("Иранский Риал", Double.parseDouble(Jsoup.connect("https://wise.com/ru/currency-converter/ils-to-kzt-rate").userAgent("Mozilla/5.0").get().selectFirst("span.text-success").text().replace(",","."))));
    currencies.put("KZT", new Currency("Казахстанский Тенге", Double.parseDouble(Jsoup.connect("https://wise.com/ru/currency-converter/ils-to-kzt-rate").userAgent("Mozilla/5.0").get().selectFirst("span.text-success").text().replace(",","."))));  
  }
  catch (Exception e){
    System.out.println("---ОШИБКА СЕТИ---");
  }
  currencies.put("NIS", new Currency("shekel", 1));
  return currencies;
  }

  private double conventToNis(double amount){ //создаем метод для перевода выбранной валюты в шекели для дальнейшего перевода
    double resultInNis = amount / priceVsNis;
    return resultInNis;
  }

  private double conventFromNis(double resultInNis){ //создаем метод для перевода из шекелей для перевода в требуемую валюту
    return resultInNis * priceVsNis;
}
  

  

  public static void main(String[] args) { //в main рома напишет основной код(с моей хелпой)
    Scanner scan = new Scanner(System.in);
    Map<String, Currency> currencies = Currency.createMap();
    if(currencies.size() != 4) {
      System.out.println("Не все курсы валют загрузились.");
      return;
    }
    System.out.println("С какой валюты нужен перевод?");
    String from = scan.nextLine().toUpperCase();
    System.out.println("В какую валюту нужен перевод?");
    String to = scan.nextLine().toUpperCase();
    System.out.println("В каком количестве?");
    double amount = scan.nextDouble();
    scan.close();
    Currency fromCurrency = currencies.get(from);
    Currency toCurrency = currencies.get(to);

    if (fromCurrency == null || toCurrency == null) {
      System.out.println("Одна из валют не найдена.");
      return;
    }
    double amountInNis = fromCurrency.conventToNis(amount);
    double finalAmount = toCurrency.conventFromNis(amountInNis);

    System.out.printf("Результат: %.2f %s → %.2f %s\n", amount, from, finalAmount, to);
    }
  }
