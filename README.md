# Car-availability-and-Rental-request-system-

📌 1. Project Overview
This system allows:
Users → Check available cars & request booking
Admin → Add cars, approve/reject requests
🧩 2. Modules
👤 User
Register / Login
View available cars
Send rental request
View booking status
🛠️ Admin
Add / update cars
View requests
Approve / reject booking
🗂️ 3. Technologies
Frontend: HTML, CSS
Backend: Python (Flask)
Database: MySQL / SQLite
🧱 4. Project Structure
Copy code

car_rental/
│
├── app.py
├── database.db
├── templates/
│   ├── index.html
│   ├── login.html
│   ├── register.html
│   ├── cars.html
│   ├── booking.html
│
├── static/
│   └── style.css
💻 5. Backend Code
from flask import Flask, render_template, request, redirect
import sqlite3

app = Flask(__name__)

# Home page
@app.route('/')
def home():
    return render_template('index.html')

# View cars
@app.route('/cars')
def cars():
    conn = sqlite3.connect('database.db')
    cur = conn.cursor()
    cur.execute("SELECT * FROM cars WHERE available=1")
    cars = cur.fetchall()
    conn.close()
    return render_template('cars.html', cars=cars)

# Booking request
@app.route('/book', methods=['POST'])
def book():
    car_id = request.form['car_id']
    name = request.form['name']

    conn = sqlite3.connect('database.db')
    cur = conn.cursor()
    cur.execute("INSERT INTO bookings (name, car_id, status) VALUES (?, ?, ?)",
                (name, car_id, 'Pending'))
    conn.commit()
    conn.close()

    return "Booking Request Sent!"

if __name__ == '__main__':
    app.run(debug=True)
🗄️ 6. Database (SQLite)
CREATE TABLE cars (
    id INTEGER PRIMARY KEY,
    name TEXT,
    price INTEGER,
    available INTEGER
);

CREATE TABLE bookings (
    id INTEGER PRIMARY KEY,
    name TEXT,
    car_id INTEGER,
    status TEXT
);
🌐 7. Frontend 
<h2>Available Cars</h2>

{% for car in cars %}
  <div>
    <h3>{{ car[1] }}</h3>
    <p>Price: {{ car[2] }}</p>

    <form action="/book" method="post">
      <input type="hidden" name="car_id" value="{{ car[0] }}">
      <input type="text" name="name" placeholder="Your Name" required>
      <button type="submit">Book</button>
    </form>
  </div>
{% endfor %}
🚀 8. Features You Can Add (For More Marks)
Login system (user & admin)
Car images
Booking history
Payment integration
Email confirmation
Search & filter cars
📊 9. ER Diagram (Text Idea)
Entities:
User (user_id, name, email)
Car (car_id, name, price, status)
Booking (booking_id, user_id, car_id, status)
Relations:
User → Booking (1:M)
Car → Booking (1:M)

🔥 10. How to Run
Bash
Copy code
pip install flask
python app.py