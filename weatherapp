#get modules
import tkinter as tk
import requests
import time
import newsapi
from newsapi import NewsApiClient
from PIL import Image, ImageTk
import os
import webbrowser
import matplotlib.pyplot as plt
import numpy as np
global city

#news api stuff 1
newsapi = NewsApiClient(api_key="b889e1c1eb674cdd9620bd526355b923")

#news api stuff 2
def getNews(app, city):

    #news search
    search_params = {
        "q": city,
        "language": "en",
        "country": "us"
    }

    #get news for cities
    news_data = newsapi.get_top_headlines(q=search_params["q"], language=search_params["language"], country=search_params["country"])

    #get news n put into string
    news = []
    for article in news_data["articles"]:
        news.append(article["title"])

    return "\n\n".join(news)

#api get weather
def getWeather(app):
    global city

    #city name
    city = textfield.get()

    #call api
    api = "http://api.openweathermap.org/data/2.5/weather?q=" + city + "&limit=20&appid=547e4e1e7c99ee57e3ed55b51a3085a2"
    json_data = requests.get(api).json()

    #get display info
    condition = json_data["weather"][0]["main"]
    temperature = int(json_data["main"]["temp"] - 273.15)
    humidity = json_data["main"]["humidity"]
    wind = json_data["wind"]["speed"]
    sunrise = time.strftime("%I:%M:%S", time.gmtime(json_data["sys"]["sunrise"] - 21600))
    sunset = time.strftime("%I:%M:%S", time.gmtime(json_data["sys"]["sunset"] - 21600))

    #display weather info
    displayinfo = condition + "\n" + str(temperature) + " °C"
    displaydata = "\n" + "Humidity: " + str(humidity) + "%" + "\n" + "Wind Speed: " + str(wind) + " km/h" + "\n" + "Sunrise: " + str(sunrise) + " AM" + "\n" + "Sunset: " + str(sunset) + " PM"
    label1.config(text=displayinfo)
    label2.config(text=displaydata)

    news = getNews(app, city)

    news_label.config(text=news)

    # Update the scrollregion of the news canvas
    news_canvas.configure(scrollregion=news_canvas.bbox("all"), height=news_canvas.winfo_height())

# Open URL in web browser
def callback(url):
    webbrowser.open_new(url)

# Create image slideshow
def create_image_slideshow(images_path, links):

    # Load images from directory
    images = []
    for filename in os.listdir("C:/Users/Teddy.Cao/OneDrive - Great Lakes Christian High School/pythonProject"):
        if filename.endswith('.jpg') or filename.endswith(".png") :
            image = Image.open(os.path.join("C:/Users/Teddy.Cao/OneDrive - Great Lakes Christian High School/pythonProject", filename))
            images.append(ImageTk.PhotoImage(image))

    # Show images on canvas
    canvas_width = 500
    canvas_height = 300
    canvas = tk.Canvas(app, width=canvas_width, height=canvas_height)
    canvas.pack()

    delay = 3000  # milliseconds between slides
    idx = 0
    # Show first image
    canvas.create_image(0, 0, anchor="nw", image=images[idx])

    # Function to show next image
    def show_next_image():
        nonlocal idx
        idx = (idx + 1) % len(images)
        canvas.itemconfig(image_item, image=images[idx])
        app.after(delay, show_next_image)

    # Start slideshow
    image_item = canvas.create_image(0, 0, anchor="nw", image=images[0])
    app.after(delay, show_next_image)

#background stuff
app = tk.Tk()
app.geometry("1920x1080")
app.title("SJ's Love Story")

# Font stuff
font1 = ("calibri", 20, "bold")
font2 = ("calibri", 40, "bold")

#serach bar
textfield = tk.Entry(app, font=font2)
textfield.pack(pady=20)
textfield.focus()
textfield.bind("<Return>", getWeather)

label1 = tk.Label(app, font=font2)
label1.pack()
label2 = tk.Label(app, font=font1)
label2.pack()

#news tab
news_frame = tk.Frame(app)
news_frame.pack(side="right", fill="both", expand=True)

#background stuff
news_canvas = tk.Canvas(news_frame, width=500)
news_scrollbar = tk.Scrollbar(news_frame, orient="vertical", command=news_canvas.yview)
news_scrollbar.pack(side="right", fill="y")

#configure scroll bar
news_canvas.pack(side="left", fill="both", expand=True)
news_canvas.configure(yscrollcommand=news_scrollbar.set)
news_canvas.bind("<Configure>", lambda e: news_canvas.configure(scrollregion=news_canvas.bbox("all")))

#new canvas for news
news_frame_inside_canvas = tk.Frame(news_canvas)

#put news into canvas
news_canvas.create_window((0, 0), window=news_frame_inside_canvas, anchor="nw")

#get nwes
news = getNews(app, textfield.get())

#display news stuff
news_label = tk.Label(news_frame_inside_canvas, text=news, font=font1, wraplength=500, justify="left")
news_label.pack(side="top", fill="both", expand=True)

location = city
url = f"https://api.openweathermap.org/data/2.5/forecast?q=" + str(city) + "&appid=547e4e1e7c99ee57e3ed55b51a3085a2&units=metric"
response = requests.get(url)
# get the temperature data from the response
data = response.json()
temps = [item['main']['temp'] for item in data['list']]
dates = [item['dt_txt'] for item in data['list']]

# convert the dates to a format that can be plotted
x = [i for i in range(len(dates))]
x_ticks = [dates[i] for i in range(0, len(dates), 8)]

# plotting the points
plt.plot(x, temps, color='green', linestyle='dashed', linewidth=3,
         marker='o', markerfacecolor='blue', markersize=12)

# limit decimals from appearing on x axis
plt.xticks(np.arange(min(x), max(x)+1, 8), x_ticks, rotation=45)

# set x and y axis range
plt.ylim(min(temps)-1, max(temps)+1)
plt.xlim(0, len(dates))

# name the x axis
plt.xlabel('Date')
# name the y axis
plt.ylabel('Temperature C')


# give a title to the graph
plt.title('Temperature forecast for ' + location)

# save the created figure to a PNG (will overwrite itself)
plt.savefig('graph.png')

# show the plot
# load the graph image using PIL
graph_image = Image.open('graph.png')

# convert the image to a Tkinter-compatible format
tk_image = ImageTk.PhotoImage(graph_image)

# create a label widget to display the image
label = tk.Label(app, image=tk_image)
label.pack()

#start program
app.mainloop()
