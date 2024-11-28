# Source-code
SOURCE CODE

from flask import Flask, render_template, request, redirect, url_for, session, flash
import sqlite3
import os

app = Flask(_name_)
app.secret_key = "your_secret_key"
UPLOAD_FOLDER = 'static/uploads'
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

# Initialize database
def init_db():
    with sqlite3.connect("database.db") as conn:
        conn.execute("""
            CREATE TABLE IF NOT EXISTS users (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                name TEXT,
                username TEXT UNIQUE,
                password TEXT,
                role TEXT
            )
        """)
        conn.execute("""
            CREATE TABLE IF NOT EXISTS products (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                

name TEXT,
                image_path TEXT,
                quantity INTEGER,
                brand TEXT,
                price INTEGER,
                details TEXT
            )
        """)
        conn.execute("""
            CREATE TABLE IF NOT EXISTS payments (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                user_id INTEGER,
                products TEXT,
                total_amount INTEGER,
                FOREIGN KEY(user_id) REFERENCES users(id)
            )
        """)
        conn.commit()

# Seed admin credentials
def seed_admin():
    with sqlite3.connect("database.db") as conn:
        try:
            conn.execute("INSERT INTO users (name, username, password, role) VALUES (?, ?, ?, ?)",
                         ("Admin", "admin", "admin123", "admin"))
            conn.commit()
        


except sqlite3.IntegrityError:
            pass

@app.route('/')
def home():
    return redirect(url_for('login'))
