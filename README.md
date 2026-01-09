import sqlite3

# Create or connect to a database
def create_connection():
    conn = sqlite3.connect('movie_database.db')
    return conn

# Create a table for movies
def create_table():
    conn = create_connection()
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS movies (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            title TEXT NOT NULL,
            genre TEXT NOT NULL,
            release_year INTEGER NOT NULL,
            director TEXT NOT NULL,
            rating REAL NOT NULL
        )
    ''')
    conn.commit()
    conn.close()

# Insert a movie into the database
def add_movie(title, genre, release_year, director, rating):
    conn = create_connection()
    cursor = conn.cursor()
    cursor.execute('''
        INSERT INTO movies (title, genre, release_year, director, rating)
        VALUES (?, ?, ?, ?, ?)
    ''', (title, genre, release_year, director, rating))
    conn.commit()
    conn.close()

# Display all movies
def view_movies():
    conn = create_connection()
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM movies')
    movies = cursor.fetchall()
    conn.close()
    if not movies:
        print("No movies found.")
    else:
        for movie in movies:
            print(f"ID: {movie[0]}, Title: {movie[1]}, Genre: {movie[2]}, Year: {movie[3]}, Director: {movie[4]}, Rating: {movie[5]}")

# Search for a movie by title
def search_movie_by_title(title):
    conn = create_connection()
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM movies WHERE title LIKE ?', ('%' + title + '%',))
    movies = cursor.fetchall()
    conn.close()
    if not movies:
        print("No movies found with that title.")
    else:
        for movie in movies:
            print(f"ID: {movie[0]}, Title: {movie[1]}, Genre: {movie[2]}, Year: {movie[3]}, Director: {movie[4]}, Rating: {movie[5]}")

# Update movie details
def update_movie(id, title, genre, release_year, director, rating):
    conn = create_connection()
    cursor = conn.cursor()
    cursor.execute('''
        UPDATE movies 
        SET title = ?, genre = ?, release_year = ?, director = ?, rating = ?
        WHERE id = ?
    ''', (title, genre, release_year, director, rating, id))
    conn.commit()
    conn.close()

# Delete a movie
def delete_movie(id):
    conn = create_connection()
    cursor = conn.cursor()
    cursor.execute('DELETE FROM movies WHERE id = ?', (id,))
    conn.commit()
    conn.close()

# Menu to interact with the movie database
def menu():
    while True:
        print("\nMovie Database Menu:")
        print("1. Add a movie")
        print("2. View all movies")
        print("3. Search for a movie by title")
        print("4. Update a movie")
        print("5. Delete a movie")
        print("6. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            title = input("Enter movie title: ")
            genre = input("Enter genre: ")
            release_year = int(input("Enter release year: "))
            director = input("Enter director: ")
            rating = float(input("Enter rating: "))
            add_movie(title, genre, release_year, director, rating)

        elif choice == '2':
            view_movies()

        elif choice == '3':
            title = input("Enter title to search: ")
            search_movie_by_title(title)

        elif choice == '4':
            id = int(input("Enter the movie ID to update: "))
            title = input("Enter new title: ")
            genre = input("Enter new genre: ")
            release_year = int(input("Enter new release year: "))
            director = input("Enter new director: ")
            rating = float(input("Enter new rating: "))
            update_movie(id, title, genre, release_year, director, rating)

        elif choice == '5':
            id = int(input("Enter the movie ID to delete: "))
            delete_movie(id)

        elif choice == '6':
            print("Exiting the program.")
            break

        else:
            print("Invalid choice! Please try again.")

if __name__ == "__main__":
    create_table()
    menu()
![moviedatabase](https://github.com/user-attachments/assets/fc0ce037-ddb9-4e27-a687-f719cf374336)
