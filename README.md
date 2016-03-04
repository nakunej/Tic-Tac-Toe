# Tic-Tac-Toe
//This is a game of Tic-Tac-Toe created on October 19th, 2015.  Acknowledgment to phD student Jeff Luo  from the University of Waterloo for template
// -------------------------------------------------------------------
// Biomedical Engineering Program
// Department of Systems Design Engineering
// University of Waterloo

// 
// I declare that, other than the acknowledgements listed below, 
// this program is my original work.
//
// Acknowledgements:
// <Jeff Luo - Provided Template>
// -------------------------------------------------------------------



using System;

// Class Program is a procedural-programming implementation of the game of tic-tac-toe.
// Player is a string [X or O].
// Player type is a string [ai or human]
// Location is a string [a1, a2, a3, b1, b2, b3, c1, c2, or c3].

static class Program
{
    // Each variable holds the mark made in the cell at the corresponding location on the game board.
    static string a1, a2, a3, b1, b2, b3, c1, c2, c3;
    
    // The player types.
    static string xPlayerType, oPlayerType;
    
    // A random number generator for use by AI players.
    static Random rGen = new Random( );
    
    //--------------------
    // Main plays the game by calling other methods.
    static void Main( )
    {
        string currentPlayer;
        string location;
        
        Console.WriteLine( );
        Console.WriteLine( "Welcome to BME121 Tic-Tac-Toe" );
        
        ClearBoard( );
        WriteBoard( );
        
        xPlayerType = GetPlayerType( "X" );
        oPlayerType = GetPlayerType( "O" );
        
        currentPlayer = GetFirstPlayer( );
        
        while( ! IsFullBoard( ) )
        {
            location = GetNextLocation( currentPlayer );
            
            MarkBoard( currentPlayer, location );
            WriteBoard( );
            
            if( IsWinForPlayer( currentPlayer ) )
            {
                Console.WriteLine( );
                Console.WriteLine( "Player {0} wins! ({1})", currentPlayer, WinPatterns( currentPlayer ) );
                return;
            }
            
            currentPlayer = GetNextPlayer( currentPlayer );
        }
        
        Console.WriteLine( );
        Console.WriteLine( "Game is a draw (no winner)." );
    }
    
    // Board-related methods
    
    //--------------------
    // Set every board cell to blank, ready for play.
    static void ClearBoard( )
    {
        a1 = " "; a2 = " "; a3 = " ";
        b1 = " "; b2 = " "; b3 = " ";
        c1 = " "; c2 = " "; c3 = " ";
    }
    
    //--------------------
    // Mark the board for a given player at a given location.
    // Invalid locations are ignored.
    static void MarkBoard( string player, string location )
    {
        switch (location)
		{
			case "a1":
				a1 = player;
				break;
			case "a2":
				a2 = player;
				break;
			case "a3":
				a3 = player;
				break;
			case "b1":
				b1 = player;
				break;
			case "b2":
				b2 = player;
				break;
			case "b3":
				b3 = player;
				break;
			case "c1":
				c1 = player;
				break;
			case "c2":
				c2 = player;
				break;
			case "c3":
				c3 = player;
				break;
			//continue for all spaces
		}
		
    }
    
    //--------------------
    // Check whether the board is full so no more moves are possible.
    static bool IsFullBoard( )
    {
        if(
			a1 == " " ||
			a2 == " " ||
			a3 == " " ||
			b1 == " " ||
			b2 == " " ||
			b3 == " " ||
			c1 == " " ||
			c2 == " " ||
			c3 == " "  
		)
			{
				return false;
			}
		else
		{
			return true;
		}
		
        
    }

    //--------------------
    // Display the current board on the console.
    static void WriteBoard( )
    {
        // Renaming some horrible codes for box elements to something memorable
        const char hl = '\u2500'; // horizontal line     
        const char vl = '\u2502'; // vertical line       
        const char tl = '\u250c'; // top left corner     
        const char tm = '\u252c'; // top middle joint    
        const char tr = '\u2510'; // top right corner    
        const char ml = '\u251c'; // middle left joint   
        const char mm = '\u253c'; // middle middle joint 
        const char mr = '\u2524'; // middle right joint  
        const char bl = '\u2514'; // bottom left corner  
        const char bm = '\u2534'; // bottom middle joint 
        const char br = '\u2518'; // bottom right corner 
        
        // Format string for output lines that are only box edges
        // Argument index   1 0 2       3
        // Line of the form [---+---+---]
        string format1 = "   {1}{0}{0}{0}{2}{0}{0}{0}{2}{0}{0}{0}{3}";
        
        // Format string for output lines that are mostly box contents
        // Argument index   0 1  2     3     4
        // Line of the form x | x11 | x12 | x13 |
        string format2 = " {0} {1} {2} {1} {3} {1} {4} {1}";
        
        // Show the board
        Console.WriteLine( );
        Console.WriteLine( format2, " ", " ", "1", "2", "3" );  // col index labels
        Console.WriteLine( format1,       hl,  tl,  tm,  tr );  // tops of boxes
        Console.WriteLine( format2, "a",  vl,  a1,  a2,  a3 );  // game-play
        Console.WriteLine( format1,       hl,  ml,  mm,  mr );  // middles of boxes
        Console.WriteLine( format2, "b",  vl,  b1,  b2,  b3 );  // game-play
        Console.WriteLine( format1,       hl,  ml,  mm,  mr );  // middles of boxes
        Console.WriteLine( format2, "c",  vl,  c1,  c2,  c3 );  // game-play
        Console.WriteLine( format1,       hl,  bl,  bm,  br );  // bottoms of boxes
    }
    
    // Player-related methods.
    
    //--------------------
    // Ask which player should go first.
    // We retry until the user enters X or O.
    // Input is automatically converted to upper case.
    static string GetFirstPlayer( )
    {
      
        Console.WriteLine( );
		bool inputCorrect = false;
		string firstPlayer = "nothing";
		while(!inputCorrect)
		{
			Console.Write( "Who should go first? X or O: " );
            firstPlayer = Console.ReadLine( ).ToUpper();
            
            if(firstPlayer == "X" || firstPlayer == "O")
            {
                inputCorrect = true;
            }
            else
            {
                Console.WriteLine("Please enter X or O");
            }
        }
        
        return firstPlayer;
    }
    
    //--------------------
    // Request the player type (ai/human) for a given player.
    // We retry until the user enters human or ai.
    // Input is automatically converted to lower case.
    static string GetPlayerType( string player )
    {
      
        Console.WriteLine( );
		bool inputCorrect = false;
		string playerType = "not initialized";
        
		Console.Write( "Enter the player type for player " + player + ", either human or ai: " );
		while(!inputCorrect)
		{
			playerType = Console.ReadLine( );
			if(playerType =="human"|| playerType == "ai")
			{
				inputCorrect = true;
			}
			else
			{
				Console.WriteLine("You did not select human or ai, please try again: ");
			}
		
		}
        return playerType;
    }
    
   
    static bool IsHuman( string player )
    {
        switch( player )
        {
            case "X": return xPlayerType == "human";
            case "O": return oPlayerType == "human";
            default: return false;
        }
    }
    
    //--------------------
    // Given the current player, return the next player.
    // We return "X" for an invalid player.
    static string GetNextPlayer( string player )
    {
        switch( player )
        {
            case "X": return "O";
            case "O": return "X";
            default: return "X";
        }
    }

    // Win-pattern-related methods.
    
    //----------
    // Check whether a given player has won the game.
    static bool IsWinForPlayer( string player )
    {
        return WinPatterns( player ) != null;
    }
    
    //--------------------
    // Identify all winning patterns for a given player.
    // We return null if there is no winning pattern.
    static string WinPatterns( string player )
    {
        
        if(player == a1 && a1 == a2 && a2 == a3)
		{
			return "row a";
		}
		 if(player == b1 && b1 == b2 && b2 == b3)
		{
			return "row b";
		}
		 if(player == c1 && c1 == c2 && c2 == c3)
		{
			return "row c";
		}
		else if(player == a2 && a2 == b2 && b2 == c2)
		{
			return "column 2";
		}
		else if(player == a1 && a1 == b1 && b1 == c1)
		{
			return "column 1";
		}
		else if(player == a3 && a3 == b3 && b3 == c3)
		{
			return "column 3";
		}
		else if(player == a1 && a1 == b2 && b2 == c3)
		{
			return "diagnol 1";
		}
		else if(player == a3 && a3 == b2 && b2 == c1)
		{
			return "diagnol 2";
		}
		else
		{
			return null;
		}
    }
    
    // Location-related methods
    
    //--------------------
    // Check whether a string represents a valid location.
    static bool IsValidLocation( string location )
    {
        
        if(
		location == "a1"||
		location == "a2"||
		location == "a3"||
		location == "b1"||
		location == "b2"||
		location == "b3"||
		location == "c1"||
		location == "c2"||
		location == "c3"
		)
		{
			return true;
		}
		
		else
		{
			return false;
		}
    }
    
    //--------------------
    // Check whether a given location empty, i.e., not marked X or O.
    // We return false for an invalid location.
    static bool IsEmptyLocation( string location )
    {
        
        if(!IsValidLocation(location))
		{
			return false;
		}
		
		switch (location)
		{	case "a1":
				return a1 == " ";
			case "a2":
				return a2 == " ";
			case "a3":
				return a3 == " ";
			case "b1":
				return b1 == " ";
			case "b2":
				return b2 == " ";
			case "b3":
				return b3 == " ";
			case "c1":
				return c1 == " ";
			case "c2":
				return c2 == " ";
			case "c3":
				return c3 == " ";
			
			default:
				return false;
		}
		
		
    }
    
    //--------------------
    // Choose a board location at random.
    // For the required default case (which can't happen here), we return null. 
    static string GetRandomLocation( )
    {
		switch(rGen.Next(1,10))//bottom bound inclusive and upper bound exclusive
		{
			case 1:
				return "a1";
			case 2:
				return "a2";
			case 3:
				return "a3";
			case 4:
				return "b1";
			case 5:
				return "b2";
			case 6:
				return "b3";
			case 7:
				return "c1";
			case 8:
				return "c2";
			case 9:
				return "c3";
			
			
			default:
				return null;
			
		}
    }
    
    //--------------------
    // Use AI to choose a location.
    // This AI chooses randomly from the unoccupied locations.
    static string GetAIChosenLocation( )
    {
        if( IsFullBoard( ) ) throw new Exception( );
        string location = GetRandomLocation( );
        while( ! IsEmptyLocation( location ) ) 
        {
            location = GetRandomLocation( );
        }
        return location;
    }
    
    //--------------------
    // Request the next play location from a given player.
    // For a human player, we repeat the request until the user
    // enters a valid location which is currently empty.
    // Input is automatically converted to lower case.
    static string GetNextLocation( string player )
    {
        string location = "not used yet";
        
        if( IsHuman( player ) )
        {
            Console.WriteLine( );
            
            bool inputCorrect = false;
            
            while (!inputCorrect)
            {
                Console.Write( " Enter a location where you want to place your mark, eg a1, c3: " );
                location = Console.ReadLine( ).ToLower();
                
              
                
                if( IsValidLocation(location) )
                {
                    if( IsEmptyLocation(location) )
                    {
                        inputCorrect = true;
                    }
                    else
                    {
                        Console.WriteLine("Please choose an empty location");
                    }
                }
                else
                {
                    Console.WriteLine("Please enter a valid location");
                }
            }
        }
        else // AI player
        {
            Console.WriteLine( );
            Console.WriteLine( "AI player {0} is thinking ...", player );
            System.Threading.Thread.Sleep( 1000 );
            location = GetAIChosenLocation( );
            Console.WriteLine( "AI player {0} chose location: {1}", player, location );
            System.Threading.Thread.Sleep( 1000 );
        }
        
        return location;
    }
}
