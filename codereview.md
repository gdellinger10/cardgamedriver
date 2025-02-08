# Code Review Documentation

## Relevant Items
* Documentation
* Code Structure
* Variables
* Loops and Conditional Statements
* General Programming Practices

## Documentation Review
* The program does not have correct or proper Javadoc in almost all classes.

### Blackjack.java
***
`public void reset`
```
/***
* Reset the deck by schuffling it.
* @param newDeck
*/
```

`public void deal()`
```
/***
* Method to deal cards to both dealer and player.
*/
```
`public boolean playerTurn()`
```
/***
* Method to add a card to the players hand if the value is less than 16, and return a boolean depending if the value is greater or equal to 21.
* @return boolean
*/
```
`public boolean dealerTurn()`
```
/***
* Method to peform the same function that playerTurn does but on dealerHand. 
* @return boolean
*/
```
`public String toString()`
```
/***
* toString method to return an organized print statement of the both the players and dealers hand and total.
* @return String
*/
```

### Deck.java
***
`public void build()`
```
/***
* Method to add a card to the deck.
*/
```
`public void schuffle()`
```
/***
* Method to randomly schuffle the cards.
*/
```
`public Card pick(int i)`
```
/***
* Method to pick a card at a given index and remove it from the deck.
* @param i
* @return picked
*/
```
`public String toString()`
```
/***
* Method to return a string of every card in the deck.
* @return deckString
*/
```

### Hand.java
***
`public int getTotalValue()`
```
/***
* Method to calculate the value of the hand.
* @return value
*/
```
`public String toString()`
```
/***
* toString method to return a string of the current cards in the hand.
* @return handString
*/
```

### Card.java
***
`public Card()`
```
/***
* Constructor to create a random card.
*/
```
`public int compareTo(Card otherCard)`
```
/***
* compareTo method to compare which card value is greater
* @param otherCard
* @return int
*/
```

### CardGameDriver.java
***
*No javadoc comments really needed*

### LamarckianPoker.java
***
`public void reset(boolean newDeck)`
```
/*** 
* Method to reset the deck, create a new deck and schuffle it.
* @param newDeck
*/
```
`public void deal()`
```
/***
* Method to deal cards to both players hands.
*/
```
`public void makePool()`
```
/***
* Add a card to the pool.
*/
```
`public boolean turn()`
```
/***
/* Method to play a turn of Lamarkian Poker.
/ @return boolean
*/
```

## Code Review
***

### Card.java
***
*Looks to implement everything that is asked of it in the README.md and doesn't seem to have any noticeable flaws.*

### Deck.java
***
*Would implement clear at the start of the build() method, in order to prevent build being called multiple times and then having a deck of more than 52 cards.*
```
    public void build() {
        clear()
        for (Card.Suit suit : Card.Suit.values()) {
            for (Card.Rank rank : Card.Rank.values()) {
                deck.add(new Card(suit, rank));
            }
        }
    }
```

*Would make sure that their is a bound check for the pick card method, as a index value could currently be picked that is out of bounds and cause an error.*
```
    public Card pick(int i) {
        if (i > 0 || i <= deck.size())
            Card picked = deck.remove(i);
            return picked;
        else:
            return null;
    }
```

### Hand.java
***
*Would move public ArrayList<Card> getHand() towards the top of the code for easier readability for anyone reviewing the code.*

*README.md doesn't ask for a public int size() method, but I don't know if this is necessarily a bad thing or not.*

*Would initiate the values fo the getTotalValue in an array then have the code check the value based off the array instead of going through each case.*
```
public int getTotalValue(){
    private static final int[] = rankValues{11, 2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10};

    int value = 0;
    int aces = 0;

    for (Card card: hand){
        int cardValue = rankValues[card.getRank().ordinal()];
        if (card.getRank() == Rank.ACE){
            aces++;
        } else{
            value += cardValue
        }
    }
    for (int i = 0; i < aces; i++){
        if (value + 11 <= 21){
            value += 11;
        } else {
            value += 1;
        }
    return value;
    }
}
```

### Blackjack.java
***
*Currently reset doesn't reset the hands, only the deck. So would add in the reset for the hand so no old cards are brought in.*
```
    public void reset(boolean newDeck) {
        if (newDeck) {
            deck = new Deck();
            deck.shuffle();
            playerHand = new Hand();
            dealerHand = new Hand()
        }
    }
```
*Can simplify the deal method, by simplying creating a loop instead of having to repeat the same code twice.*
```
    public void deal() {
        playerHand = new Hand();
        dealerHand = new Hand();
        for (int i = 0; i < 2; i++){
            playerHand.addCard(deck.deal());
            dealerHand.addCard(deck.deal());
        }
    }
```
*Would also improve the output of the toString method as it seems very clunky at the moment.*
```
    public String toString() {
        String result = "Blackjack Hands:\n";
        result += "Players Hand: \n;
        for (int i = 0; i < playerHand.size(); i++){
            result += playerHand.getCard(i) + " + ";
        }
        result += "Player's Total: " + playerHand.getTotalValue() + "\n";
        result += "Dealer's Hand: \n"
        for (int i = 0; i < dealerHand.size(); i++){
            result += dealerHand.getCard(i) + " + ";
        }
        result += "Dealer's Total: " + dealerHand.getTotalValue() + "\n";
        return result;
    }
```
*I would also move the scoring seen in the CardGameDriver.java to Blackjack.java class as it makes more sense to handle that scoring and results within the Blackjack class.*
```
        while (iGame < NGAMES) {
            game.deal();
            if (game.getPlayerHand().getTotalValue() == 21) {
                playerWins++;
            } else if (game.getDealerHand().getTotalValue() == 21) {
                dealerWins++;
            } else {
                boolean playerResult = game.playerTurn();
                boolean dealerResult = game.dealerTurn();
                if (!playerResult) {
                    dealerWins++;
                } else if (!dealerResult) {
                    playerWins++;
                } else if (game.getPlayerHand().getTotalValue() < game.getDealerHand().getTotalValue()) {
                    dealerWins++;
                } else if (game.getPlayerHand().getTotalValue() > game.getDealerHand().getTotalValue()) {
                    playerWins++;
                }
            }
            if (game.getDeck().size() < 10) {
                game.reset(true);
            }

            iGame++;
        }
```

### LamarckianPoker.java
***
*Like the blackjack class, the reset class doesn't fully reset all game functions mainly the hands. So adding a simple reset after the if statement could stop any issues.*
```
    public void reset(boolean newDeck) {
        if (newDeck) {
            deck = new Deck();
            discard = new Deck();
            discard.clear();
            deck.shuffle();
        }
        iTurn = 0;
        player1Hand = new Hand();
        player2Hand = new Hand();
    }
```
*Starting with the turn method, create a variable for the 7 hand size so it's not a magic number.*
```
    private static final MAX_HAND_SIZE = 7
    if (player1Hand.size() < MAX_HAND_SIZE || player2Hand.size() < MAX_HAND_SIZE) {}
```
*The error that is being caused by the LamarckianPoker.java is from line 62, having a bound must be postive error. I believe the reason for this is that one of the hands has above 7 cards, which satisfies the if statement. Than when Java attempts to draw a card, there is no card to be drawn as the other players hand has no cards avaliable. To fix this I think adding a check to see if each player has a card to be drawn will fix the issue.*
```
   public boolean turn() {
        if (player1Hand.size() < 7 || player2Hand.size() < 7) {
            makePool();
            // System.out.println("Turn " + iTurn + "\n" + pool);
            if (player1Hand.size() == 0 || player2Hand.size() == 0){
                System.out.println("Game over, player is out of cards.)
                return false;x
            }
            Card player1Card = player1Hand.getCard(rand.nextInt(player1Hand.size()));
            Card player2Card = player2Hand.getCard(rand.nextInt(player2Hand.size()));
            Hand firstHand, secondHand;
            Card firstCard, secondCard;
    }
```
### CardGameDriver.java
***
*The only think I would change is removing the scoring for blackjack being done in the driver program and change it over to the Blackjack.java class.