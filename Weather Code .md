weather.html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Прогноз погоды</title>

    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>

    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <style>
        body {
            background: linear-gradient(to right, #9bc9ff, #e7f3ff);
            min-height: 100vh;
        }
        .weather-card {
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            background: white;
            padding: 20px;
            margin-top: 20px;
        }
        
        .day-card {
            margin-top: 20px;
            border-radius: 10px;
        }
        .forecast-scroll {
            display: flex;
            overflow-x: auto;
            gap: 10px;
            padding-bottom: 10px;
        }
        .forecast-mini {
            flex: 0 0 auto;
            min-width: 130px;
            background: #f8f9fa;
            border-radius: 8px;
            border: 1px solid #ddd;
            padding: 10px;
            font-size: 13px;
            text-align: center;
        }
        .accordion-button.collapsed::after {
            transform: rotate(0deg);
        }
        .accordion-button::after {
            transition: transform 0.2s;
        }
        .city-list {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 20px;
        }
        .city-btn {
            border: 2px solid #0d6efd;
            padding: 8px 16px;
            border-radius: 8px;
            background-color: white;
            cursor: pointer;
            font-weight: 500;
            transition: 0.3s;
        }
        .city-btn:hover {
            background-color: #0d6efd;
            color: white;
            transform: scale(1.05);
        }
    </style>

    <!-- Скрипт даты -->
    <script>
    document.addEventListener("DOMContentLoaded", function () {
        const dateElement = document.getElementById("current-date");
        const today = new Date();
        const formattedDate = today.toLocaleDateString("ru-RU"); // ДД.ММ.ГГГГ
        if (dateElement) {
            dateElement.innerHTML += formattedDate;
        }
    });
    </script>

    <!-- Скрипт с эмодзи -->
    <script>
    document.addEventListener("DOMContentLoaded", function () {
        const emojiMap = {
            "ясно": "☀️",
            "облачно": "☁️",
            "пасмурно": "🌥️",
            "дожд": "🌧️",
            "гроза": "⛈️",
            "снег": "❄️",
            "туман": "🌫️",
            "ветер": "💨"
        };

function addEmoji(text) {
    const lower = text.toLowerCase();
    for (const key in emojiMap) {
        if (lower.includes(key)) {
            console.log("Сработал ключ:", key)
            return text + " " + emojiMap[key];
        }
    }
    return text;
}


        document.querySelectorAll("p").forEach(p => {
            if (p.innerText.includes("Описание:")) {
                const parts = p.innerText.split(":");
                p.innerText = parts[0] + ": " + addEmoji(parts[1].trim());
            } else {
                for (const key in emojiMap) {
                    if (p.innerText.toLowerCase().includes(key)) {
                        p.innerText = addEmoji(p.innerText);
                    }
                }
            }
        });
    });
    </script>
    
</head>
<body class="container py-4">

    <h1 class="text-center mb-4 fw-bold">Прогноз погоды</h1>

<nav class="navbar navbar-expand-lg bg-white shadow-sm mb-4" style="border-radius: 8px;">
  <div class="container-fluid d-flex flex-wrap">

    <span class="navbar-brand fw-bold me-3">Популярные города:</span>

    <ul class="navbar-nav d-flex flex-wrap gap-2">
      <li class="nav-item">
        <form method="GET">
          <input type="hidden" name="city" value="Москва">
          <input type="hidden" name="units" value="{{ units }}">
          <button class="btn btn-outline-primary">Москва</button>
        </form>
      </li>
      <li class="nav-item">
        <form method="GET">
          <input type="hidden" name="city" value="Алматы">
          <input type="hidden" name="units" value="{{ units }}">
          <button class="btn btn-outline-primary">Алматы</button>
        </form>
      </li>
      <li class="nav-item">
        <form method="GET">
          <input type="hidden" name="city" value="Париж">
          <input type="hidden" name="units" value="{{ units }}">
          <button class="btn btn-outline-primary">Париж</button>
        </form>
      </li>
      <li class="nav-item">
        <form method="GET">
          <input type="hidden" name="city" value="Лондон">
          <input type="hidden" name="units" value="{{ units }}">
          <button class="btn btn-outline-primary">Лондон</button>
        </form>
      </li>
      <li class="nav-item">
        <form method="GET">
          <input type="hidden" name="city" value="Дубай">
          <input type="hidden" name="units" value="{{ units }}">
          <button class="btn btn-outline-primary">Дубай</button>
        </form>
      </li>
      <li class="nav-item">
        <form method="GET">
          <input type="hidden" name="city" value="Сидней">
          <input type="hidden" name="units" value="{{ units }}">
          <button class="btn btn-outline-primary">Сидней</button>
        </form>
      </li>
    </ul>
  </div>
</nav>

<div class="card p-4 shadow-sm">
    <form method="GET" class="row g-3">
        <div class="col-md-6">
            <input type="text" name="city" class="form-control" placeholder="Введите город" value="{{ request.GET.city }}">
        </div>
        <div class="col-md-3">
            <select name="units" class="form-select">
                <option value="metric" {% if units == "metric" %}selected{% endif %}>°C</option>
                <option value="imperial" {% if units == "imperial" %}selected{% endif %}>°F</option>
                <option value="standard" {% if units == "standard" %}selected{% endif %}>K</option>
            </select>
        </div>
        <div class="col-md-3">
            <button type="submit" class="btn btn-primary w-100">Узнать</button>
        </div>
    </form>
</div>

{% if weather %}
<div class="weather-card">
<h3 id="current-date">{{ weather.city }} — </h3>
    <p><strong>Температура:</strong> {{ weather.temp }}
        {% if units == "imperial" %}°F{% elif units == "standard" %}K{% else %}°C{% endif %}
    </p>
    <p><strong>Ощущается как:</strong> {{ weather.feels_like }}
    {% if units == "imperial" %}°F{% elif units == "standard" %}K{% else %}°C{% endif %}
</p>
    <p><strong>Влажность:</strong> {{ weather.humidity }}%</p>
    <p><strong>Давление:</strong> {{ weather.pressure }} hPa</p>
    <p><strong>Ветер:</strong> {{ weather.wind }} м/с</p>
    <p><strong>Описание:</strong> {{ weather.desc }}</p>
</div>
{% endif %}

{% if forecast %}
<h3 class="mt-5">Прогноз по дням</h3>

<div class="accordion" id="forecastAccordion">
    {% for date, items in forecast.items %}
    <div class="accordion-item mb-2 shadow-sm">
        <h2 class="accordion-header" id="heading{{ forloop.counter }}">
            <button class="accordion-button collapsed" type="button"
                    data-bs-toggle="collapse"
                    data-bs-target="#collapse{{ forloop.counter }}"
                    aria-expanded="false"
                    aria-controls="collapse{{ forloop.counter }}">
                📅 {{ date }}
            </button>
        </h2>
        <div id="collapse{{ forloop.counter }}"
             class="accordion-collapse collapse"
             aria-labelledby="heading{{ forloop.counter }}"
             data-bs-parent="#forecastAccordion">
            <div class="accordion-body">
                <div class="forecast-scroll">
                    {% for item in items %}
                    <div class="forecast-mini">
                        <p class="mb-1"><strong>{{ item.dt_txt|slice:"11:16" }}</strong></p>
                        <p class="mb-1">{{ item.main.temp }}{% if units == "imperial" %}°F{% elif units == "standard" %}K{% else %}°C{% endif %}</p>
                        <p class="mb-1">{{ item.weather.0.description }}</p>
                    </div>
                    {% endfor %}
                </div>
            </div>
        </div>
    </div>
    {% endfor %}
</div>

<div class="weather-card mt-5">
    <h3 class="text-center mb-4">Средняя температура по дням</h3>
    <canvas id="tempChart" height="120"></canvas>
</div>

<script>
    document.addEventListener("DOMContentLoaded", function () {
        const forecastData = {{ forecast|safe }};
        const labels = [];
        const averages = [];

        for (const [date, items] of Object.entries(forecastData)) {
            let sum = 0;
            let count = 0;
            items.forEach(item => {
                sum += item.main.temp;
                count++;
            });
            labels.push(date);
            averages.push((sum / count).toFixed(1));
        }

        const unitLabel = {
            "metric": "°C",
            "imperial": "°F",
            "standard": "K"
        }["{{ units }}"];

        const ctx = document.getElementById("tempChart").getContext("2d");
        new Chart(ctx, {
            type: "line",
            data: {
                labels: labels,
                datasets: [{
                    label: `Температура (${unitLabel})`,
                    data: averages,
                    tension: 0.3,
                    pointRadius: 5,
                    borderWidth: 2
                }]
            },
            options: {
                responsive: true,
                scales: {
                    y: {
                        beginAtZero: false
                    }
                }
            }
        });
    });
</script>
{% endif %}

{% if error %}
<div class="alert alert-danger mt-3">
    {{ error }}
</div>
{% endif %}

</body>
</html>





views.py
from django.http import HttpResponse
import requests
from django.shortcuts import render

API_KEY = "a0298a279c2a05ed816637e8e11080d0"

def index(request):
    return HttpResponse("<h4>Проееект сделааан!</h4>")

def about(request):
    return HttpResponse("<h4>Не смотри я стесняюсь</h4>")



def weather_view(request):
    city = request.GET.get('city')
    units = request.GET.get('units', 'metric')
    selected_date = request.GET.get('date')
    weather_data = None
    forecast_data = None
    error = None

    if city:
        url = f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={API_KEY}&units={units}&lang=ru"
        response = requests.get(url)

        if response.status_code == 200:
            data = response.json()
            weather_data = {
                "city": data["name"],
                "temp": data["main"]["temp"],
                "feels_like": data["main"]["feels_like"],
                "humidity": data["main"]["humidity"],
                "pressure": data["main"]["pressure"],
                "wind": data["wind"]["speed"],
                "desc": data["weather"][0]["description"]
            }

        else:
            error = "Город не найден. Попробуйте снова."

        # Прогноз
        forecast_url = f"https://api.openweathermap.org/data/2.5/forecast?q={city}&appid={API_KEY}&units={units}&lang=ru"
        forecast_response = requests.get(forecast_url)

        if forecast_response.status_code == 200:
            forecast_json = forecast_response.json()
            forecast_list = forecast_json["list"]

            # Группировка по датам
            grouped = {}
            for item in forecast_list:
                date = item["dt_txt"].split(" ")[0]
                if date not in grouped:
                    grouped[date] = []
                grouped[date].append(item)

            forecast_data = grouped

    if not city:
        error = "Введите название города."
    return render(request, "weather.html", {
        "weather": weather_data,
        "forecast": forecast_data,
        "selected_date": selected_date,
        "error": error,
        "units": units,
    })
