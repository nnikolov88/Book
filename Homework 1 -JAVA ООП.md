//Николай Георгиев Николов 1801261006-Домашна Работа 

package library;


import java.util.ArrayList;
import java.util.InputMismatchException;
import java.util.Scanner;
 
public class Book {
 
	public String title;
	public String author;
	public String publisher;
	public String publicationYear;
	public String status;
	public String borrower;
	public String borrowDate;
	public String returnDate;
 
	public String status1 = "Available";
	public String status2 = "Borrowed";
	public int BookChoice;
 
 
	static ArrayList<String> UserList = new ArrayList<String>();
	static ArrayList<Book> BookList = new ArrayList<Book>();
 
	static int choice ;
 
	static Scanner userInput = new Scanner(System.in);
	static Scanner choiceInput = new Scanner(System.in);

 
	public static void displayFirstMenu(){
		System.out.println(">########################################################################");
		System.out.println("> Izberete edna ot slednite opcii kato posochite neiniqt nomer (naprimer 2): ");
		System.out.println(">====================================================================");
		System.out.println("2- Dobavqne na nova kniga v bibliotekata.");
		System.out.println("6- Iztrivane na cqlata biblioteka.");
		System.out.println("7- Vrushtane v glavnoto menu.");
		System.out.println("0- Exit.");
		System.out.println(">########################################################################");
		System.out.println(">  Vuvedete vashiq izbor tuk (naprimer 2): ");
		choice = choiceInput.nextInt();
 
	}
 
	public static void displaySecondMenu(){
		System.out.println(">########################################################################");
		System.out.println("> Izberete edna ot slednite opcii kato posochite neiniqt nomer (naprimer 2): ");
		System.out.println(">====================================================================");
		System.out.println("1- Proverka na spisuka s nalichnite knigi.");
		System.out.println("2- Dobavqne na nova kniga v bibliotekata.");
		System.out.println("3- Zaemane na kniga.");
		System.out.println("4- Vrushtane na kniga.");
		System.out.println("5- Iztrivane na kniga.");
		System.out.println("6- Iztrivane na cqlata biblioteka.");
		System.out.println("7- Vrushtane v glavnoto menu.");
		System.out.println("0- Exit.");
		System.out.println(">########################################################################");
		System.out.println("> Vuvedete vashiq izbor tuk (naprimer 2): ");
		choice = choiceInput.nextInt();
 
	}
 
	public String displayBook(){
 
		String BookInfo = "----------------------------"+
						"\nTitle:.................."+title+
						"\nAuthor:................."+author+
						"\nPublisher:.............."+publisher+ 
						"\nPublicationYear:........"+publicationYear+
						"\nStatus:................."+status+
						"\nBorrower:..............."+borrower+
						"\nDate Borrowed:.........."+borrowDate+
						"\nReturn date:............"+returnDate+
						"\n----------------------------";
		return BookInfo;	
	}
 
	public void createBook(){
		System.out.println("> Vuvedete zaglavie na knigata: ");
		title = userInput.nextLine();
 
		System.out.println("> Vuvedete avtor na knigata: ");
		author = userInput.nextLine();
 
		System.out.println("> Vuvedete izdatelstvo na knigata: ");
		publisher = userInput.nextLine();
 
		System.out.println("> Vuvedete Godina na izdavane na knigata: ");
		publicationYear = userInput.nextLine();
 
		borrower = "nobody";
		borrowDate = "none";
		returnDate = "none";
 
		status = "Available";
	}
 
	public void addBook(){
		Book newBook = new Book(); 
		newBook.createBook();
		BookList.add(newBook);//dobavqne na kniga v spisuka.
		System.out.println("---------------------------------------------------------");
		System.out.println("> Uspeshno dobavihte kniga v bibliotekata!\n");
		System.out.println("---------------------------------------------------------");	
	}
 
	public void displayBookList(){
		if (BookList.size() == 0){
			System.out.println(">-------------------------------------------------------------");
			System.out.println("> Bibliotekata e prezna molq dobavete kniga purvo !\n");
			System.out.println(">-------------------------------------------------------------");
			Book.displayFirstMenu();
			choice = choiceInput.nextInt();
 
		} else {					
			for (int i = 0; i < BookList.size(); i++){
				System.out.printf("\n>-----------Book Index: [%s]---------------------------------\n",i+1);
				System.out.println(BookList.get(i).displayBook());	
				System.out.println(">-------------------------------------------------------------");
			}	
		}	
	}
 
	public void borrowBook(){
		System.out.println("---------------------------------------------------------");
		System.out.println("> Tuk sa vsichki knigi koito sa nalichni v bibliotekata: ");
		System.out.println("---------------------------------------------------------");		
		displayBookList();
 
		borrowLoop1:
		while(choice == 3){
			System.out.println("\n\n> Choose an available book from the above list and write down it's index number: ");
			BookChoice = choiceInput.nextInt()-1;
			if(BookChoice > BookList.size()){
				System.out.println("> Nomerut na knigata koqto izbrahte ne e v spisuka!");
				choice = 7;
			}else if(BookChoice <= BookList.size()){
				break borrowLoop1;
			}
		}		
 
		borrowLoop2:
		while(choice == 3){
			
			if (BookList.get(BookChoice).status.equalsIgnoreCase(status1) && BookList.size() >= BookChoice){
				
				BookList.get(BookChoice).status = "Borrowed";
				System.out.printf("\n> You have chosen the following book: %s\n", BookList.get(BookChoice).displayBook());
 
				
				BookList.get(BookChoice).borrower = borrower;
				BookList.get(BookChoice).borrowDate = "Today.";
				BookList.get(BookChoice).returnDate = "In two weeks.";
				System.out.println("> Trqbva da vurnete knigata to 2 sedmici!");
				choice = 7;
				break borrowLoop2;
 
			}else if(BookList.get(BookChoice).status.equalsIgnoreCase(status2) && BookList.size() >= BookChoice){
				System.out.println("> Knigata koqto obitahte da vzemete e nedostupna vmomenta!");
				choice = 7;
				break borrowLoop2;
			}else if(BookChoice > BookList.size()-1){
				System.out.println("> Nomerut koito vuvedahte ne e nalichen v spisuka s knigi!");
				choice = 7;
				break borrowLoop2;
			}
		}
	}
 
 
	public void returnBook(){
		System.out.println("> Vuvedete zaglavieto na knigata koqto iskate da vurnete: ");
		String returnBookTitle = userInput.nextLine();
		int x = 0;
		while (x < BookList.size()){
									
			if (BookList.get(x).title.equalsIgnoreCase(returnBookTitle)){
				BookList.get(x).status = "Available";
				BookList.get(x).borrower = "none";
				BookList.get(x).borrowDate = "none";
				System.out.println("> Uspeshno vurnahte knigata v bibliotekata!");
				Book.displayFirstMenu();
				choice = choiceInput.nextInt();
				break;
			}
			x = x+1;
		}
		x = 0;
		while (x < BookList.size() && BookList.size() > 0){
									
			if (BookList.get(x).title.equalsIgnoreCase(returnBookTitle)){
		}else{
			System.out.println("> The are no books with this title in the library." +
					" Please make sure the title is spelt correctly or choose to add the book " +
					"to the library from the main menu. ");
			Book.displayFirstMenu();
			choice = choiceInput.nextInt();				
			}
		}
		Book.displayFirstMenu();
		choice = choiceInput.nextInt();
	}
 
 
	public void removeBook(){
		int i = 0;
		System.out.println("---------------------------------------------------------");
		System.out.println("> Tuk sa vsichki nalichni knigi v bibliotekata: ");
		System.out.println("---------------------------------------------------------");
 
		while (i < BookList.size() && BookList.size() > 0){
			System.out.printf("\n> Book number %s:\n%s",i+1,BookList.get(i));
			i = i+1;
		}
 
		System.out.println("\n\n> Choose an available book from the above list and write down it's number: ");
		int BookChoice = choiceInput.nextInt()-1;
 
		while(choice == 5){
			try{
				if (BookChoice > 0 && BookChoice < BookList.size() && BookList.get(BookChoice).status.equalsIgnoreCase("Available")){
					
					BookList.remove(BookChoice);
					System.out.printf("\n> You have chosen to delete the following book: %s\n", BookList.get(BookChoice));
					System.out.printf("\n> The Book %s is deleted.\n", BookList.get(BookChoice));
					choice = 7;
				}
			}catch(InputMismatchException error){
				System.out.println("<ERROR> Molq vuvedete nomer na knigata ot spisuka: ");
				choiceInput.nextInt();
				choice = 5;
			}catch(IndexOutOfBoundsException error){
				System.out.println("<ERROR> Molq vuvedete nomer na knigata ot spisuka: ");
				choice = 5;
			}
		}		
	}
 
 
	public void emptyLibrary(){
		System.out.println("> WARNING < Vie izbrahte da iztriete vsichki knigi ot bibliotekata! ");
		System.out.println("> Siguren li ste?? Vuvedete 'yes' ili 'no' za da produljite: ");
		String confirmation = userInput.nextLine();
		try{
			if(confirmation.equalsIgnoreCase("yes")){
				System.out.println("> Bibliotekata se iztriva molq izchakaite...");
				BookList.clear();
				System.out.println("> Bibliotekata e prazna!");
				choice = 7;
			}
		}catch(InputMismatchException error){
			System.out.println("<ERROR> Molq uverete se che ste napisali 'yes' ili 'ne' pravilno! : ");
			choice = 6;
		}
	}
 
 
	public void addUser(){
		System.out.println("> Vuvedete vasheto ime: ");
		borrower = userInput.nextLine();
		UserList.add(borrower);	
	}
 
	public void run(){
 
		System.out.println("@TEST@ <<< 1 >>>>");
 
		addUser();
		System.out.println("@TEST@ <<< 2 >>>>");
 
		Book.displayFirstMenu();
 
		System.out.println("@TEST@ <<< >>>>");
 
		exit:
 
			while(choice != 0){	
				try{
					
					if(choice == 1 && BookList.size() > 0){
 
						displayBookList();
						choice = 7;
					}
 
					if(choice == 1 && BookList.size() == 0){
						System.out.println("<ERROR> Bibliotekata e prazna! Molq dobavete kniga purvo!");
						choice = 7;
					}
					
					if(choice == 2){
						
						addBook();
						displaySecondMenu();
					}
				
					if(choice == 3){
						if(BookList.size() > 0){
							borrowBook();							
						}						
					}
				
					if(choice == 4){
						returnBook();
					}
					
					if(choice == 5){
						removeBook();
						try{
							if(BookList.size() > 0){
								displaySecondMenu();
							}
						}catch(IndexOutOfBoundsException error){
							System.out.println("<ERROR> Molq vuvedete kniga purvo!");
							choice = 7;
							
						}
					}
				
					if(choice == 6){
						emptyLibrary();						
					}
					
					if(choice == 7){
						if(BookList.size() > 0){
							displaySecondMenu();
						}else if(BookList.size() == 0){							
							displayFirstMenu();
						}
					}
				
					if(choice == 0){
						break exit;
					}
				}catch(InputMismatchException error){				
					System.out.println("@TEST@ <<<  >>>>");
					break exit;
				}
 
			}
 
		System.out.println("#### Vie izlqzohte ot bibliotekata!  ####");
 
		}
 
 
	
 
	public static void main(String[] args){
 
		System.out.println("> Dobre doshli v bibliotekata!");
 
		Book newBook = new Book();
		newBook.run();
 
	}
