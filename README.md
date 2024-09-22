#include <stdio.h>
#include <string.h>
#include <math.h>
#include <time.h>
#include <stdlib.h>

// MAUREEN ESTRADA
//DIANA LEE CELSTRA
// SMANTHA ILLEDAN
//CASSANDRA GUDALLE7

float shuffle(float deck[]);
void convertCard(float card);
float randomCard();
int analyzeHand(int player, float deck[]);
void printHandScore(int score);
float* sortHand(float hand[]);
void printResults(int scores[], int players, float deck[]);
int tieBreaker(float deck[], int scores[], int tieBreakerValue, int ties, int players[]);
int detHighestCard(float deck[], int amountofTies, int players[], int rank);
void delay(int seconds);
float currentDeck[52];
int scores[10];
int players = 0;
int i,j,k;

int distribution(void){

    system("cls");
    system("COLOR 20");

	printf("\n\n\n\t\t\tHow many players? There can be a maximum of 9 players\n");

	printf("\n\n\n\t\t\t");

	scanf("%d", &players);


	if(players > 9){
		printf("Too Many Players, please try again. Up to 9 can play.");
		exit(0);
	}

	printf("\n");

	srand(time(NULL));
	shuffle(currentDeck);

	int debug = 0;
	if(debug == 1){
		currentDeck[0] = 14.2;
		currentDeck[1] = 2.4;
		currentDeck[2] = 3.1;
		currentDeck[3] = 2.2;
		currentDeck[4] = 10.1;
		currentDeck[5] = 6.1;
		currentDeck[6] = 5.3;
		currentDeck[7] = 4.4;
		currentDeck[8] = 5.3;
		currentDeck[9] = 12.2;
		currentDeck[10] = 9.4;
		currentDeck[11] = 3.1;
		currentDeck[12] = 6.1;
		currentDeck[13] = 5.2;
		currentDeck[14] = 10.4;
		currentDeck[15] = 5.2;
		currentDeck[16] = 7.2;
		currentDeck[17] = 11.2;
		currentDeck[18] = 9.1;
		currentDeck[19] = 8.1;
		currentDeck[20] = 12.1;
		currentDeck[21] = 13.1;
		currentDeck[22] = 6.1;
		currentDeck[23] = 2.1;
		currentDeck[24] = 14.4;

	}

	system("cls");
	printf("\n\n\t\tDistributing Hands to Players");
	delay(1);
	for(i=0; i < 3; i++){
		printf(".");
		delay(1);
	}

	printf("\n\n");

	k = 0;
	for(i=0; i<=players; i++){
		if(i == 0)
			printf("Here is the Dealer's Hand: ");
		else
			printf("Here is Player %d's Hand: ", i);
		for(j=k; j<k+5; j++){

			convertCard(currentDeck[j]);

		}
		k = k + 5;
		printf("\n");
	}

	char choice;
        printf("\n\n\t\t\tDo you want to play or pass?");
    printf("\n\n\t\t\t[P] for Play [S] for Pass\n\t\t\t");
    scanf("\n\t\t\t %c", &choice);
    if (choice == 'P' || choice == 'p'){
    calculation();
    }
    else{
    printf("\n\n\t\t\t Thank you for playing!");
                    printf("\n\n\n\n");
                    printf("\n\n\t\t\t\t\tExiting . . . .\n\n");
                    for(int i =1; i<= 120; i++)
                    {
                        _sleep(3);
                        printf("%c", 219);
                    }
                        printf("\n\n\t\t\t\tSuccessfully exited from the game. \n");
                        return 0;
}
}


int calculation(){

	int i;
	for(i=0; i<=players; i++)
		scores[i] = analyzeHand(i, currentDeck);

	printf("\nCalculating Player Scores");
	delay(1);
	for(i=0; i < 3; i++){
		printf(".");
		delay(1);
	}

	printf("\n");


	printResults(scores, players, currentDeck);
	return 0;

}

float shuffle(float deck[]){

	float currentCard;
	int i = 0;
	int j = 0;

	int duplicate = 0;

	while(i<52){
		currentCard = randomCard();


		duplicate = 0;



		for(j=0; j<52; j++){
			if(currentCard == deck[j]){
				duplicate = 1;
				break;
			}
		}


		if(duplicate == 0){
			deck[i] = currentCard;

			i++;
		}

	}
	return 0;

}

void convertCard(float card){

	int i;
	double value, suit;
	suit = round(modf(card, &value)*10);

	char cardName[100];

	cardName[0] = '\0';

    char clubs[] = "Clubs";
	char diamonds[] = "Diamonds";
	char hearts[] = "Hearts";
	char spades[] = "Spades";

	char two[] = "Two";
	char three[] = "Three";
	char four[] = "Four";
	char five[] = "Five";
	char six[] = "Six";
	char seven[] = "Seven";
	char eight[] = "Eight";
	char nine[] = "Nine";
	char ten[] = "Ten";
	char jack[] = "Jack";
	char queen[] = "Queen";
	char king[] = "King";
	char ace[] = "Ace";

	if(floor(card) == 2)
		strcat(cardName, two);
	else if(floor(card) == 3)
		strcat(cardName, three);
	else if(floor(card) == 4)
		strcat(cardName, four);
	else if(floor(card) == 5)
		strcat(cardName, five);
	else if(floor(card) == 6)
		strcat(cardName, six);
	else if(floor(card) == 7)
		strcat(cardName, seven);
	else if(floor(card) == 8)
		strcat(cardName, eight);
	else if(floor(card) == 9)
		strcat(cardName, nine);
	else if(floor(card) == 10)
		strcat(cardName, ten);
	else if(floor(card) == 11)
		strcat(cardName, jack);
	else if(floor(card) == 12)
		strcat(cardName, queen);
	else if(floor(card) == 13)
		strcat(cardName, king);
	else
		strcat(cardName, ace);

	strcat(cardName, " of ");

	if(suit == 1)
		strcat(cardName, clubs);
	else if(suit == 2)
		strcat(cardName, diamonds);
	else if(suit == 3)
		strcat(cardName, hearts);
	else if (suit == 4)
		strcat(cardName, spades);

	printf(" %s ", cardName);

}


float randomCard(){
			int value = rand()%13+2;
			int suit = rand()%4+1;
			float currentCard = (float)value + (float)suit/10;
			return currentCard;
		}


int analyzeHand(int player, float deck[]){
    int pairs = 0;
    int score = 1;
    int i;
    int j = 0;
    int currentCard;
    int suitsInHand[4] = {0};
    int facesInHand[14] = {0};
	int currentHand[5];

    for(i=0; i<4; i++)
	suitsInHand[i] = 0;
    for(i=0; i<14; i++)
	facesInHand[i] = 0;

    for(i=player*5; i<player*5+5; i++){
	currentHand[j] = deck[i];
	j++;
    }

    double value, suit;
    for(i = 0; i < 5; i++){
	currentCard = currentHand[i];
	suit = round(modf(currentCard, &value)*10);
	suitsInHand[(int)suit-1]++;
	facesInHand[(int)value-1]++;
    }

    for(i=0; i<14; i++){
	if(facesInHand[i] == 2)
		pairs++;
    }

    for(i=0; i<14; i++){
	if(facesInHand[i] == 3){
		score = 4;
		break;
	}
    }

    for(i=0; i<10; i++){
	if(facesInHand[i] == 1 && facesInHand[i+1] == 1 && facesInHand[i+2] == 1 && facesInHand[i+3] == 1 && facesInHand[i+4] == 1){
		score = 5;
		break;
	}
    }

    for(i=0; i<4; i++)
	if(suitsInHand[i] == 5){
		score = 6;
		break;
	}

    if(pairs == 1 && score == 4)
	score = 7;

    for(i=0; i<14; i++){
	if(facesInHand[i] == 4){
		score = 8;
		break;
	}
    }

    for(i=0; i<4; i++)
	if(score == 5 && suitsInHand[i] == 5)
		score = 9;

    if(score == 9 && facesInHand[9] == 1 && facesInHand[12] == 1)
	score = 10;

    if(score == 1){
	if(pairs == 1)
		score = 2;
	if(pairs == 2)
		score = 3;
    }

    printf("\nNumber of pairs: %d", pairs);
    printf("\n The score is: %d", score);
    printf("\n");
    return score;
}




void printHandScore(int score){

	switch(score){
			case 1:{
				printf("Highest Card");
				break;
			}
			case 2:{
				printf("Pair");
				break;
			}
			case 3:{
				printf("Two Pair");
				break;
			}
			case 4:{
				printf("Three of a Kind");
				break;
			}
			case 5:{
				printf("Straight");
				break;
			}
			case 6:{
				printf("Flush");
				break;
			}
			case 7:{
				printf("Full House");
				break;
			}
			case 8:{
				printf("4 of a Kind");
				break;
			}
			case 9:{
				printf("Straight Flush");
				break;
			}
			case 10:{
				printf("Royal Flush");
				break;
			}
		}



}


float* sortHand(float hand[]){


	int i, j;
	float temp;


	for(i = 4; i >= 0; i--){
		for(j = i-1; j>=0; j--)

			if(hand[i] < hand[j]){
				temp = hand[i];
				hand[i] = hand[j];
				hand[j] = temp;
				i = 4;
				}

			}

	return hand;
}


void printResults(int scores[], int players, float deck[]){
	int i, j;
	int winIndex, winner;
	int ties;


	int tiedPlayers[10];
	for(i=0; i<10; i++)
		tiedPlayers[i] = 0;


	for(i = 0; i <= players; i++){
		if(i == 0)
			printf("\nDealer scored a ");
		else
			printf("\nPlayer %d scored a ", i);

		printHandScore(scores[i]);
	}


	winIndex = 0;

	for(i = 0; i <=players; i++)
		if(scores[i] > scores[winIndex])
			winIndex = i;

	ties = -1;

	for(i = 0; i <=players; i++)
		if(scores[i] == scores[winIndex]){
			tiedPlayers[i] = 1;
			ties++;
		}

	printf("\n");

	if(ties == 0){
		if(winIndex == 0)
			printf("\nDealer wins with a ");
		else
			printf("\nPlayer %d wins with a ", winIndex);

		printHandScore(scores[winIndex]);
		printf("!!!");
	}

	else{
		if(ties == 1)
			printf("\nThere is a tie!!\n");
		else
		printf("\n%d Players are tied!!!\n", ties+1);

		winner = tieBreaker(deck, scores, scores[winIndex], ties, tiedPlayers);



		if(winner != -1){
			printf("\nThe winner of the Tie Breaker for a ");
			printHandScore(scores[winIndex]);
			if(winner == 0)
				printf(" is the Dealer!!!");
			else
				printf(" is Player %d!!!", winner);
		}
		if(winner == -1)
			printf("\nThere is no winner, all those that tied win!!!");

	}
}


int tieBreaker(float deck[], int scores[], int tieBreakerValue, int ties, int players[]){


	int winner;

	int amountofTies = ties + 1;

	int i;


	switch(tieBreakerValue){
			case 1:
			case 2:
			case 3:
			case 4:
			case 5:
			case 6:
			case 7:
			case 8:
			case 9:{
				winner = detHighestCard(deck, amountofTies, players, tieBreakerValue);
				break;
			}

			case 10:{
				printf("No tiebreaker possible, all those that tied win!");
				winner  = -1;
				break;
			}
	}

	return winner;

}


int detHighestCard(float deck[], int amountofTies, int players[], int rank){


	int	i, j, k,winner, highestCard, sum = 0, highestSum = 0, multiTierTie = 0;
	float randHand[5];
	printf("\n");



	printf("These Players are in the Tie Breaker for a ");
	printHandScore(rank);
	printf(":");
	j=0;
	for(i=0; i<10; i++){
		if(players[i] == 1){
			if(i != 0)
				printf(" Player %d", i);
			else
				printf(" Dealer");

			if(j<amountofTies-2){
				printf(",");
				j++;
			}
			else if(j == amountofTies-2){
				printf(" &");
				j++;
			}
		}
	}


		printf("\n\nCalculating Tie Breaker Results");
		delay(1);
		for(i=0; i < 3; i++){
			printf(".");
			delay(1);
		}

	printf("\n");


	if(rank == 1 || rank == 5 || rank == 6 || rank == 9){

		for(i=0; i<10; i++)
			if(players[i] == 1){
				for(j = 0; j<5; j++)
					randHand[j] = deck[j+i*5];

				sortHand(randHand);
				highestCard = randHand[4];
				break;
			}

		i=0;
		while(i < 10){

			if(players[i] != 1){

				i++;
				continue;
			}

			for(j=i*5; j<i*5+5; j++){
				if((int)deck[j] >= (int)highestCard){
					highestCard = deck[j];

					winner = i;

				}
			}
			i++;
		}

		for(i=0; i<10; i++)
			if(players[i] == 1){
				for(j=i*5; j<i*5+5; j++)
					if((int)deck[j] == (int)highestCard){
						players[i] = 2;
						break;
					}
			}


		for(i=0; i<10; i++)
			if(players[i] == 2)
				multiTierTie++;


		k = 3;

		if(rank == 1 || rank == 5){
			while(multiTierTie > 1){

				if(k == -1)
					return -1;

				highestSum = 0;
				multiTierTie = 0;

				highestSum = 0;
				for(i=0; i<10; i++)
					if(players[i] == 2){
						for(j = 0; j<5; j++)
							randHand[j] = deck[j+i*5];

						sortHand(randHand);

						for(j=4; j>=k; j--)
							highestSum = highestSum + (int)randHand[j];
						winner = i;
						break;
						}



				while(i<10){
					sum = 0;
					if(players[i] == 2){
						for(j = 0; j<5; j++)
							randHand[j] = deck[j+i*5];

						sortHand(randHand);
						for(j=4; j>=k; j--)
							sum = sum + (int)randHand[j];


						if(sum == highestSum){
							multiTierTie++;
							players[i] = 2;
						}
						if(sum > highestSum){
							highestSum = sum;
							winner = i;

						}
						if(sum < highestSum){
							players[i] = 0;
						}

						}
					else{

				}
					i++;
				}

				k--;
			}

		}
		if(rank == 6 || rank == 9 && multiTierTie > 1)
			return -1;


	}


	if(rank == 2 || rank == 3 || rank == 4 || rank == 7 || rank == 8){
		int highestPair = 0;
		int highest3OAK = 0;
		int highest4OAK = 0;
		int pairs = 0;
		winner = 0;
		int check = 0;


		if(rank == 2 || rank == 3){

			for(i=0; i<10; i++)
				if(players[i] == 1){
					for(j = 0; j<5; j++)
						randHand[j] = (int)deck[j+i*5];

					sortHand(randHand);
					for(j=0; j<5; j++){

						if(randHand[j] == (int)randHand[j+1]){


							if(randHand[j] == highestPair){

								if(rank == 2)
									winner = -1;
								if(rank == 3 && check == 1){

									winner = -1;
								}
								if(rank == 3 && check == 0){

									j = 0;
									check = 1;
								}

							}

							if(randHand[j] > highestPair){
							highestPair = randHand[j];
							winner = i;

							}

						}
					}
				}
			}


		if(rank == 4 || rank == 7 || rank  == 8){

			for(i=0; i<10; i++)
				if(players[i] == 1){
					for(j = 0; j<5; j++)
						randHand[j] = (int)deck[j+i*5];

					sortHand(randHand);
					for(j=0; j<5; j++){

						if(rank == 8)
							if(randHand[j] == randHand[j+1] && randHand[j] == randHand[j+2] && randHand[j] == randHand[j+3]){

								if(randHand[j] == highest4OAK){

									winner = -1;
								}

								if(randHand[j] > highest4OAK){
									highest4OAK = randHand[j];
									winner = i;

								}
							}


						if(rank == 4 || rank == 7){
							if(randHand[j] == randHand[j+1] && randHand[j] == randHand[j+2]){

								if(randHand[j] == highest3OAK){

									winner = -1;
								}

								if(randHand[j] > highest3OAK){
									highest3OAK = randHand[j];
									winner = i;

								}

							}
						}
					}
				}
		}
		}
	return winner;
}

void delay(int seconds){

	int milli_seconds = 1000 * seconds;

	clock_t start = clock();

	while(clock() < start + milli_seconds){
	}
}

void how(){
    system("cls");
    system("COLOR 20");
    printf("\n\t\t\t\t FLOW OF THE GAME");
    printf("\n\t\t\t\t 1. Enter how many players (Max of 9)");
    printf("\n\t\t\t\t 2. 5 cards will be dealt");
    printf("\n\t\t\t\t 3. You'll have the option to play or pass");
    printf("\n\t\t\t\t 4. Showdown (Card combination of both players will be shown)");
    printf("\n\t\t\t\t 5. Winner will be announced");
    }

int main()
{
  char choice, ans;
    system("COLOR 4E");
    printf("\n\n\n\t\t\t\t==========   ==========  ||      ||   =========");
    printf("\n\t\t\t\t||               ||      ||      ||   ||      \\\\");
    printf("\n\t\t\t\t||               ||      ||      ||   ||       \\\\\\");
    printf("\n\t\t\t\t||               ||      ||      ||   ||        \\\\\\");
    printf("\n\t\t\t\t==========       ||      ||      ||   ||        ///");
    printf("\n\t\t\t\t\t||       ||      ||      ||   ||       ///");
    printf("\n\t\t\t\t\t||       ||      ||      ||   ||      //");
    printf("\n\t\t\t\t\t||       ||      ||      ||   ||     //");
    printf("\n\t\t\t\t==========       ||       ========    =========");
    printf("\n\n\t\t\t\t  ======   =====  ||  //   ======   =======");
    printf("\n\t\t\t\t ||    || ||   || || //    ||       ||    ||");
    printf("\n\t\t\t\t ||    || ||   || ||//     ||       ||    ||");
    printf("\n\t\t\t\t ||    || ||   || ||\\\\     ||====   ||    ||");
    printf("\n\t\t\t\t  ======  ||   || || \\\\    ||       =======");
    printf("\n\t\t\t\t ||       ||   || ||  \\\\   ||       ||    \\\\");
    printf("\n\t\t\t\t ||       ||   || ||   \\\\  ||       ||     \\\\");
    printf("\n\t\t\t\t ||        =====  ||    \\\\ ======   ||      \\\\");

    printf("\n\n\t\t\t\t START [G]AME \n\t\t\t\t [F]LOW OF THE GAME \n\t\t\t\t [E]xit");
    printf("\n\t\t\t\t [G] [F] or [E]?\n\t\t\t\t\t");
    fflush(stdin);
    scanf(" %c", &choice);

    if(choice == 'g'  || choice == 'G') {
    distribution();}

    else if(choice == 'f' || choice == 'F') {
            how();}

    else if (choice == 'e' || choice == 'E'){
    printf("\n\n\n\n");
    printf("\n\n\t\t\t\t\tExiting . . . .\n\n");
    for(int i = 1; i <= 120; i++) {
    _sleep(3);
    printf("%c", 219);
    }}

    else{
    printf("\n\n\t\t\t\tInvalid choice. Please try again.\n\n");
    }

    return 0;
}







