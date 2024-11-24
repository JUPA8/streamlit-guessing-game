import streamlit as st  # Import Streamlit to create the app's user interface
import random  # We'll use random to pick an animal and generate a number for the game
import pandas as pd  # Import pandas to help with statistics management
import matplotlib.pyplot as plt  # We’ll use matplotlib to show a bar chart for game stats
import openai  # OpenAI library for future use (could be used for GPT or AI-related tasks)

# Don't forget to replace this with your actual OpenAI API key
openai.api_key = "YOUR_OPENAI_API_KEY"

# Check if game statistics are already in session state, if not, initialize them
if 'games_played' not in st.session_state:
    st.session_state.games_played = 0  # Counter for how many games have been played
if 'total_guesses' not in st.session_state:
    st.session_state.total_guesses = 0  # Counter for total guesses made across all games
if 'guesses_per_game' not in st.session_state:
    st.session_state.guesses_per_game = []  # List to track the number of guesses per game

# This function resets the game, especially useful for restarting a new game
def reset_game():
    st.session_state.number = random.randint(1, 100)  # Random number for the game, simulating a "guess"
    st.session_state.guesses = 0  # Reset guesses for the new game

# Sidebar navigation where the user can choose between playing the game or viewing stats
st.sidebar.title("Navigation")  # Title for the sidebar
page = st.sidebar.radio("Choose a page", ("Play", "Stats"))  # Options to choose either "Play" or "Stats"

# This part of the code runs when the user chooses the "Play" option
if page == "Play":
    st.title("Guess the Animal Game")  # Display the title of the game
    st.write("Think of an animal and I will try to guess it by asking questions!")  # Instructions for the player

    # If the game hasn't started yet, initialize some variables
    if 'animal' not in st.session_state:
        st.session_state.animal = None  # Placeholder for the chosen animal (later assigned randomly)
        st.session_state.guesses = []  # List to hold the questions and answers
        st.session_state.is_playing = True  # Flag to indicate the game is active
        st.session_state.question_index = 0  # Tracks which question the user is on
        st.session_state.questions = [
            "Is it a mammal?",  # These are the questions asked during the game
            "Is it larger than a human?",
            "Does it live in water?",
            "Is it a domesticated animal?",
            "Does it have four legs?",
            "Is it a carnivore?",
            "Is it found in Africa?"
        ]

    # Button to start a new game
    if st.button("Start New Game"):
        reset_game()  # Reset the game when the button is clicked
        # Randomly choose an animal from the list
        st.session_state.animal = random.choice(["dog", "cat", "elephant", "tiger", "whale"])
        st.session_state.guesses = []  # Reset guesses list
        st.session_state.question_index = 0  # Reset the question index to start from the first question
        st.session_state.is_playing = True  # Start the game
        st.write("I'm thinking of an animal. Please answer my yes/no questions!")  # Instructions to the player

    # If the game is still in progress, ask the questions
    if st.session_state.is_playing:
        if st.session_state.question_index < len(st.session_state.questions):
            question = st.session_state.questions[st.session_state.question_index]  # Get the next question
            answer = st.selectbox(question, ("Yes", "No"))  # Ask the question and get a response (Yes/No)

            # Save the question and answer pair for later
            st.session_state.guesses.append((question, answer))

            # Button to move to the next question
            if st.button("Next Question"):
                st.session_state.question_index += 1  # Move to the next question

            # Once all questions are answered, make a final guess
            if st.session_state.question_index == len(st.session_state.questions):
                st.write(f"I guess your animal is a {st.session_state.animal}!")  # Final guess based on the answers
                st.session_state.games_played += 1  # Update the number of games played
                st.session_state.total_guesses += len(st.session_state.guesses)  # Add the number of guesses to the total
                st.session_state.guesses_per_game.append(len(st.session_state.guesses))  # Store this game's guesses
                st.session_state.is_playing = False  # End the game
                st.session_state.question_index = 0  # Reset the question index for the next game
        else:
            st.write("Game Over! Please start a new game.")  # If all questions are asked, let the user know the game is over

# If the user selects the "Stats" page, show their game statistics
elif page == "Stats":
    st.title("Game Statistics")  # Title of the stats page
    st.write(f"Total games played: {st.session_state.games_played}")  # Show total number of games played
    if st.session_state.games_played > 0:
        # Calculate the average number of guesses per game
        average_guesses = st.session_state.total_guesses / st.session_state.games_played
        st.write(f"Average guesses per game: {average_guesses:.2f}")

        # Create a bar chart to show how many guesses were made per game
        fig, ax = plt.subplots()  # Create a figure and axis for the chart
        ax.bar(range(len(st.session_state.guesses_per_game)), st.session_state.guesses_per_game)  # Plot the guesses
        ax.set_xlabel("Games")  # Label for the x-axis
        ax.set_ylabel("Number of Guesses")  # Label for the y-axis
        ax.set_title("Guesses per Game")  # Title of the chart
        st.pyplot(fig)  # Display the chart
    else:
        st.write("No games played yet.")  # If no games have been played, tell the user
