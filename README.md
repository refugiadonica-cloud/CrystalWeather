package com.example.crystalweather.weather.model

enum class TemperatureUnit(val apiValue: String, val symbol: String) {
    CELSIUS("celsius", "°C"),
    FAHRENHEIT("fahrenheit", "°F")
}

enum class WindSpeedUnit(val apiValue: String, val label: String) {
    KMH("kmh", "km/h"),
    MS("ms", "m/s"),
    MPH("mph", "mph")
}

data class SavedCity(
    val id: String,
    val name: String,
    val region: String = "",
    val country: String = "",
    val latitude: Double,
    val longitude: Double,
    val isCurrentLocation: Boolean = false
) {
    val displaySubtitle: String
        get() = listOf(region, country).filter { it.isNotBlank() }.joinToString(", ")
}

data class CurrentWeather(
    val timeIso: String,
    val temperature: Double,
    val apparentTemperature: Double,
    val humidityPercent: Int,
    val windSpeed: Double,
    val pressureHpa: Double?,
    val uvIndex: Double?,
    val weatherCode: Int,
    val isDay: Boolean
)

data class HourlyForecast(
    val timeIso: String,
    val temperature: Double,
    val precipitationProbability: Int?,
    val weatherCode: Int,
    val isDay: Boolean
)

data class DailyForecast(
    val dateIso: String,
    val weatherCode: Int,
    val tempMax: Double,
    val tempMin: Double,
    val precipitationProbabilityMax: Int?,
    val sunriseIso: String?,
    val sunsetIso: String?,
    val uvIndexMax: Double?
)

data class WeatherData(
    val cityName: String,
    val latitude: Double,
    val longitude: Double,
    val timezone: String,
    val temperatureUnit: TemperatureUnit,
    val windSpeedUnit: WindSpeedUnit,
    val current: CurrentWeather,
    val hourly: List<HourlyForecast>,
    val daily: List<DailyForecast>,
    val fetchedAtEpochMillis: Long
)

sealed interface WeatherResult<out T> {
    data class Success<T>(val data: T) : WeatherResult<T>
    data class Error(val message: String, val cause: Throwable? = null) : WeatherResult<Nothing>
}
