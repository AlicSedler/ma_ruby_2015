## ������� ��� ������ �������� ������

������� ������� ��������� ���� ������(���������: ������ ������� �������� ������������) ������ �������� ������.

��� ������� ���������:
* ���������� ������ � �������� ������;
* ���������� ��� ��������;
* ���������� ��������� ������ ��������;

> � ������ �������� ��������������� ���������, 
> ���� ��� ��������� � ������ ���������.

## ������ ������

��� ������ � �������� ������ ��������������� ������� �����:
```ruby
WeatherService # �� ��������� �������
GismeteoWeatherService : WeatherService
SinoptikWeatherService : WeatherService
YahooWeatherService : WeatherService
WezzooWeatherService : WeatherService
```

```ruby
WeatherService # ����������� ����, ���� �� ������� �������� ������:
{
	boolean HasForecastFor(ForcastRange) # ����� ��� �������� �� ���� ����� ����� ���� ������ ������� ������ (�� ������, �������, 2 ����...)
	Array<WeatherForecast> GetForecastFor(City, ForcastRange) # ����� ��� ��������� ��������, � ����� ��������� ������ ������ �� ����� ��������
}
```

������� ����� WeatherService ������ ���������� ������ ���� �������� ������.

## ��� ��������
��� ������ ����� ���� ���������� ����������� ����� ForecastFetcher.

```ruby
ForecastFetcher
{
	# ����
	Array<City> cities # ������ ��� ��� ���� ���� ����������� ������. �������� ���� ������� ��� ��� ������
 
	# ������
	Array<WeatherForecast> CollectForecastsFor(WeatherService, ForcastRange) # ��������� �������� ��� ��� �������������� ������ ����� ������ �� �������� ����� ����
}
```

## �����

��� ������ ����� ����������� ���������� ���� WeatherServiceAnalyzer, � ����� ���� ���������� ����� Analyze.

```ruby
WeatherServiceAnalyzer
{
	# ����
	WeatherService service
	
    # �������� ������
	boolean LoadDataToAnalyzeFor(WeatherService)
	AnalyzeResult Analyze()
}

SimpleTemperatureWeatherServiceAnalyzer : WeatherServiceAnalyzer
{
	Array<City> cities
	
	Array<WeatherForecast> pastForecasts
	Array<WeatherForecast> todayForecasts
	
    # �������������� ���������� ������
	boolean LoadDataToAnalyzeFor(WeatherService)
	AnalyzeResult Analyze()
}

CherkassyPrecipitationWeatherServiceAnalyzer : WeatherServiceAnalyzer
{
	City cherkassyCity
	
	Array<WeatherForecast> pastForecasts
	Array<WeatherForecast> todayForecasts
	
    # �������������� ���������� ������
	boolean LoadDataToAnalyzeFor(WeatherService)
	AnalyzeResult Analyze()
}

```

## ��� � ����������� ������
��� � ����������� ������ ���� ������������ ���� ��� � ����.
�� ������ ���� ������� ���� ��������� ���� WeatherScheduler.

```ruby
WeatherScheduler
{
	WeatherUpdater updater
    
	void Serve() # � ����� ���������� ���������� � ������ ������ Tick() ��� � ����
    void Tick() # ������� ����� Update ����� WeatherUpdater
}
```

```ruby

WeatherUpdater
{
	WeatherServicesAnalyzerController analyzer
	WeatherForecastsFetcherController fetcher
	
	void Update()
}

WeatherServicesAnalyzerController
{
	Array<WeatherService> services;
	Array<WeatherServiceAnalyzer> analyzers;

	boolean LoadAvailableServices()
	boolean LoadAvailableAnalyzers()
	
	void AnalyzeAllAndSave()
}

WeatherForecastsFetcherController
{
	Array<WeatherService> services;
	Array<City> cities;
	
	ForecastFetcher fetcher;
	
	boolean LoadAvailableServices()
	boolean LoadAvailableCities()
	
	void FetchAndSave()
}
```
