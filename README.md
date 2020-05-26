# SpellingPuzzleGame
A spelling logic puzzle game based off of the New York Times Spelling Bee game. I used this code for my create project for AP CSP

    #imports
    import simplegui, random									#imports the graphics and random

    #varriables
    canvas_width = 600											#keeps track of the canvas width
    canvas_height = 400											#keeps track of the canvas height

    game_stage = "pre-game"										#keeps track if the user is in the game or not

    word_guess_list = []										#keeps track of the user's guesses
    word_list = []												#keeps track of the words the user found

    word_valid = True											#keeps track if their word is valid
    guess_not_valid_message = ""								#message to tell them if word is not valid

    score = 0													#keeps track of the user's score
    word_ammount = 0											#keeps track of how many words the user found

    game_option = 0												#keeps track of the different games the user could get
    last_game_option = 0										#keeps track of what was the last game so the user doesn't get the same one in a row
    letter_list = []											#keeps track of the list of letters for the chose option
    correct_words_choice = []									#keeps track of which list of words is chosen
    center_letter = ""											#keeps track of the center letter

    new_game_clicked_once = False

    correct_words_list1 = ["THYRISTORS","ORTHOTISTS","THYRISTOR","ORTHOTIST","HOTSHOTS","SHORTISH","ORTHOSIS","HISTORY","HOTSHOT","HOTTISH","THIRSTY",
         "HORRORS","THIRSTS","SHORTY","THIRTY","TOOTHY","HOISTS","HORROR","SHIRTS","SHOOTS","SHORTS","SOOTHS","THIRST","TOOTHS","TROTHS","HOOTY",
         "HORSY","HOTTY","HISTS","HOIST","HOOTS","HORST","HOSTS","ORTHO","SHIRT","SHOOS","SHOOT","SHORT","SHOTS","SOOTH","TOOTH","TROTH",
         "HISS","HIST","HITS","HOOT","HOST","HOTS","OOHS","SHOO","SHOT","SITH","SOYS","THIO","THIS"]
    correct_words_list2 = ["ADSTRATUM","STARDUSTS","MUSTARDS","MADRASAS","DASTARDS","STARDUST","ADSTRATA","DUMDUMS","DAMMARS","MARAUDS","MUSTARD",
         "ARMADAS","MADRASA","DASTARD","DATURAS","DUMDUM","AMDRAM","DAMMAR","MADAMS","MADRAS","DURUMS","DATUMS","MARAUD","ARMADA","DAMARS","DRAMAS","MADRAS",
         "DATURA","RADARS","MADAM","DURUM","DATUM","DRUMS","MAUDS","MUDRA","DAMAR","DRAMA","DRAMS","DUADS","RUDDS","SUDDS","ADUST","DAUTS","DURRA",
         "DURST","DUSTS","STUDS","SURDS","TURDS","DARTS","DRATS","RADAR","DRUM","MAUD","MUDS","DAMS","DRAM","MADS","DUAD","DUDS","RUDD","SUDD","ADDS",
         "DADS","DAUT","DURA","DUST","STUD","SUDS","SURD","TURD","DART","DATA","SARD"]

    #helper functions
    def set_up_game():											#function that sets up the game
        global game_stage, word_guess_list, correct_words_choice, letter_list, center_letter,correct_words_choice, word_list
        global word_valid, guess_not_valid_message, word_ammount, score
        #set up varriables
        word_valid = True										#resets if the guess is valid or not
        guess_not_valid_message = ""							#resets the guess not valid message
        correct_words_choice = []								#resets the word choice
        score = 0												#resets the score
        word_ammount = 0										#resets the ammount of words
        word_guess_list = []									#clears the word guess list
        word_list = []											#clears the word_list
        game_stage = "in game"									#sets the game stage to in game
        #calls the function that chooses the game
        choose_game_option()				

    def choose_game_option():									#function that chooses which game
        global last_game_option, game_option
        game_option = random.randrange(1,3)						#chooses a random game
        while last_game_option == game_option:
            game_option = random.randrange(1,3)
            print("last_game_option: ", last_game_option)
            print("game_option: ", game_option)
        if game_option == 1:									#game option one
            game_option_1()
        elif game_option == 2:									#game option two
            game_option_2()

    def game_option_1():										#function that sets up game option 1
        global last_game_option, letter_list, center_letter, correct_words_choice
        correct_words_choice = correct_words_list1.copy()		#sets what the correct words are
        letter_list = ["H","I","S","T","O","R","Y"]				#sets the letters for the list
        center_letter = letter_list[0]							#figures out what the center letter is
        last_game_option = 1
        print("Game Chosen: ", game_option)
        print(" ")	

    def game_option_2():										#function that sets up game option 2
        global last_game_option, letter_list, center_letter, correct_words_choice
        correct_words_choice = correct_words_list2.copy()		#sets what the correct words are
        letter_list = ["D","A","T","S","M","U","R"]				#sets the letters for the list
        center_letter = letter_list[0]							#figures out what the center letter is
        last_game_option = 2
        print("Game Chosen: ", game_option)
        print(" ")	    

    def update_word_list():										#function that updates the word list
        global word_list, guess_not_valid_message, word_valid, word_ammount, score
        if (word_guess_list[len(word_guess_list)- 1] not in word_list) and (word_guess_list[len(word_guess_list)- 1] in correct_words_choice):
            word_valid = True
            word_list.append(word_guess_list[len(word_guess_list)- 1])
            #updates the word ammount
            word_ammount = word_ammount + 1
            #updates the score
            score = score + len(word_guess_list[len(word_guess_list)- 1])- 3
            #checks if all letters are in the word to give an extra seven points
            num = 0
            for i in range(len(letter_list)):
                if word_guess_list[len(word_guess_list)- 1].find(letter_list[i]) != -1:
                    num = num + 1
                if num >= 7:
                    score = score + 7
            print("SCORE: ", score)
            print(" ")
        elif word_guess_list[len(word_guess_list)- 1] in word_list:					#sees if the word has already been found
            word_valid = False
            guess_not_valid_message = "Already Played"
            print(guess_not_valid_message)
        elif len(word_guess_list[len(word_guess_list)- 1]) < 4:						#sees if their word is too short
            word_valid = False
            guess_not_valid_message = "Too Short"
            print(guess_not_valid_message)
        elif word_guess_list[len(word_guess_list)- 1].find(center_letter) == -1:	#sees if it is missing the center letter
            word_valid = False
            guess_not_valid_message = "Mising Center Letter"
            print(guess_not_valid_message)
        elif word_guess_list[len(word_guess_list)- 1] not in correct_words_choice:	#sees if the word is not in the correct words list
            word_valid = False
            guess_not_valid_message = "Not in Word List"
            print(guess_not_valid_message)
        print("Word list: ", word_list)

    #event handlers
    def word_input_handler(word_input):							#function that takes in the user's input
        global word_guess_list
        word_guess_list.append(word_input.upper())
        print("Word guess list: ", word_guess_list)
        update_word_list()										#calls the function to update the word list

    def button_new_game_handler():								#function that is called when new game button is pressed
        global new_game_clicked_once
        print("---------------------NEW GAME---------------------")
        set_up_game()											#function that sets up the new game
        if new_game_clicked_once == False:
            frame.add_button ("RESTART", button_restart_handler, 80)		#creates the buttton "RESTART"
            frame.add_button ("MAIN MENU", button_main_menu_handler, 80)	#creates the button "MAIN MENU"
            frame.add_button ("EXIT", button_exit_handler, 80)				#creates the button "EXIT"
            frame.add_input("WORD", word_input_handler,80)					#creates the input "WORD"
        new_game_clicked_once = True

    def button_restart_handler():								#function that is called when new game button is pressed
        global word_guess_list, game_stage, word_valid,guess_not_valid_message, word_list, word_ammount, score
        if game_stage == "pre-game":
            pass
        else:
            print("---------------------RESTART----------------------")
            #resets varraibles
            game_stage = "in game"									#keeps track if the user is in the game or not

            word_guess_list = []									#keeps track of the user's guesses
            word_list = []											#keeps track of the words the user found

            word_valid = True										#keeps track if their word is valid
            guess_not_valid_message = ""							#message to tell them if word is not valid

            score = 0												#keeps track of the user's score
            word_ammount = 0										#keeps track of how many words the user found

    def button_main_menu_handler():								#function that takes the user to the pre game screen
        global game_stage
        if game_stage == "pre-game":
            pass
        else:
            print("--------------------MAIN MENU---------------------")
            game_stage = "pre-game"

    def button_exit_handler():									#function that stops the frame
        frame.stop()											#stops the frame

    def draw(canvas):											#draw handler function
        if game_stage == "pre-game":
            #gives the instrustions on how to play the game on the start screen
            canvas.draw_text("How To Play:", (20, 50), 40, "blue")
            canvas.draw_text("You will be given a set of letters. From these letters find how many ", (50, 80), 20, "#3848eb")
            canvas.draw_text("combinations you can make of words with more than three letters", (18, 110), 20, "#3848eb")
            canvas.draw_text("that contain the middle highlighted letter. Words that do not contain", (18, 140), 20, "#3848eb")
            canvas.draw_text("the middle letter do not count. Swear words and names do not count", (18, 170), 20, "#3848eb")
            canvas.draw_text("either. You can use the same letter multiple times per word.", (18, 200), 20, "#3848eb")
            canvas.draw_text("Enter your words in the input box on the left. You earn one point", (50, 230), 20, "#3848eb")
            canvas.draw_text("for four letter words and an extra point for each additional letter. Words", (18, 260), 20, "#3848eb")
            canvas.draw_text("containing all the letters gain an extra seven points.", (18, 290), 20, "#3848eb")
            canvas.draw_text("Click new game to begin or start a new game. If you want to restart,", (50, 320), 20, "#3848eb")
            canvas.draw_text("click restart. To quit click the exit button. Good luck!", (18, 350), 20, "#3848eb")
        elif game_stage == "in game":
            #tells the user their score, number of correct words, and the words that they found
            canvas.draw_text(("SCORE: " + str(score)), (30, 380), 20, "#09751f")
            canvas.draw_text(("NUMBER OF WORDS FOUND: " + str(word_ammount)), (240, 380), 20, "#09751f")
            canvas.draw_text("Your words: ", (180, 30), 25, "Blue")
            #canvas.draw_line([160, 0], [160, canvas_height], 10, "white")
            #draws the letters in circles
            canvas.draw_circle((80, 200), 20, 2, 'black', '#f50fd2')
            canvas.draw_text(letter_list[0], (71, 210), 25, "black")
            canvas.draw_circle((80, 155), 20, 2, 'black', 'white')
            canvas.draw_text(letter_list[1], (73, 165), 25, "black")
            canvas.draw_circle((80, 245), 20, 2, 'black', 'white')
            canvas.draw_text(letter_list[2], (73, 255), 25, "black")
            canvas.draw_circle((41.03, 222.5), 20, 2, 'black', 'white')
            canvas.draw_text(letter_list[3], (33, 232), 25, "black")
            canvas.draw_circle((41.03, 177.5), 20, 2, 'black', 'white')
            canvas.draw_text(letter_list[4], (32, 187), 25, "black")
            canvas.draw_circle((118.97, 222.5), 20, 2, 'black', 'white')
            canvas.draw_text(letter_list[5], (109, 232), 25, "black")
            canvas.draw_circle((118.97, 177.5), 20, 2, 'black', 'white')
            canvas.draw_text(letter_list[6], (110, 187), 25, "black")
            #draws the user's correct words
            x = 180
            y = 60
            for i in range(0,len(word_list)):
                canvas.draw_text(word_list[i], (x, y), 20, "#a911f0")
                y = y + 20
                if y == (canvas_height - 40):
                    x = x + 130
                    y = 60
                i = i + 1
            #tells the user if their entered word does not work
            if word_valid == False:
                canvas.draw_text(guess_not_valid_message, (10, 120), 17, "#c91010")
                canvas.draw_text(word_guess_list[len(word_guess_list)- 1], (10, 100), 17, "blue")

    #set frame and create handlers
    frame = simplegui.create_frame('Spelling Puzzle', canvas_width, canvas_height)	#creates the frame
    frame.set_canvas_background("cyan")							#sets the background color of the canvas
    frame.set_draw_handler(draw)								#sets the draw handler

    frame.add_button ("NEW GAME", button_new_game_handler, 80)		#creates the buttton "NEW GAME"

    #start frame
    frame.start()												#starts the frame
