# VideoRental

````
package step1;

import java.util.ArrayList;
import java.util.Enumeration;
import java.util.Iterator;
import java.util.List;
import java.util.Vector;

class Customer {
	private String name;
	private List<Rental> rentals = new ArrayList<>();

	public Customer(String name) {
		this.name = name;
	};

	public void addRental(Rental rental) {
		rentals.add(rental);
	}

	public String getName() {
		return name;
	};

	public String statement() {
		double totalAmount = 0;
		int frequentRenterPoints = 0;
		Iterator<Rental> iterator = rentals.iterator();
		String result = "Rental Record for " + getName() + "\n";

		while ( iterator.hasNext() ) {
			double thisAmount = 0;
			Rental each = (Rental) iterator.next();
			// determine amounts for each line

			switch (each.getMovie().getPriceCode()) {
			case Movie.REGULAR:
				thisAmount += 2;
				if (each.getDaysRented() > 2)
					thisAmount += (each.getDaysRented() - 2) * 1.5;
				break;
				
			case Movie.NEW_RELEASE:
				thisAmount += each.getDaysRented() * 3;
				break;
				
			case Movie.CHILDRENS:
				thisAmount += 1.5;
				if (each.getDaysRented() > 3)
					thisAmount += (each.getDaysRented() - 3) * 1.5;
				break;
			}
			// add frequent renter points
			frequentRenterPoints++;
			// add bonus for a two day new release rental
			if ((each.getMovie().getPriceCode() == Movie.NEW_RELEASE) && each.getDaysRented() > 1)
				frequentRenterPoints++;
			// show figures
			result += "\t" +  String.valueOf(thisAmount) + "(" + each.getMovie().getTitle() + ")" + "\n";

			totalAmount += thisAmount;
		}

		result += "Amount owed is " + String.valueOf(totalAmount) + "\n";
		result += "You earned " + String.valueOf(frequentRenterPoints) + " frequent renter pointers";

		return result;
	}
}
````
````
package step1;

public class Movie {
	public static final int CHILDRENS = 2;
	public static final int REGULAR = 0;
	public static final int NEW_RELEASE = 1;
	
	private String title;
	private int priceCode;

	public Movie(String title, int priceCode) {
		this.title = title;
		this.priceCode = priceCode;
	}

	public int getPriceCode() {
		return priceCode;
	}

	public void setPriceCode(int arg) {
		priceCode = arg;
	}

	public String getTitle() {
		return title;
	};
}
````
````
package step1;

class Rental {
	private Movie movie;
	private int daysRented;

	public Rental(Movie movie, int daysRented) {
		this.movie = movie;
		this.daysRented = daysRented;
	}

	public int getDaysRented() {
		return daysRented;
	}

	public Movie getMovie() {
		return movie;
	}
}
````
````
package step1;

import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.CoreMatchers.notNullValue;
import static org.junit.Assert.*;

import org.junit.Test;

public class CustomerTest {
	private static final String CHILDRENS_MOVIE_NAME = "childrensMovieName";
	private static final String NEW_RELEASE_MOVIE_NAME = "newReleaseMovieName";
	private static final String REGULAR_MOVIE_NAME = "regularMovieName";
	Customer customer = new Customer("Customer_name");

	@Test
	public void newCustomer() {
		assertThat(customer, is(notNullValue()));
	}
	@Test
	public void customerNoRentalMovie() {
		assertThat(customer.statement(), is("Rental Record for Customer_name\n"
				+ "Amount owed is 0.0\n"
				+ "You earned 0 frequent renter pointers"));
	}
	
	@Test
	public void customerRentRegularMovie3days(){
		createRental(REGULAR_MOVIE_NAME, Movie.REGULAR, 3);

		assertThat(customer.statement(), is("Rental Record for Customer_name\n"
				+ "\t3.5(regularMovieName)\n"
				+ "Amount owed is 3.5\n"
				+ "You earned 1 frequent renter pointers"));
	}
	private void createRental(String movieTitle, int movieType, int daysRented) {
		Movie movie = new Movie(movieTitle, movieType);
		Rental rental = new Rental(movie, daysRented);
		customer.addRental(rental);
	}

	@Test
	public void customerRentNewReleaseMovie2days(){
		createRental(NEW_RELEASE_MOVIE_NAME, Movie.NEW_RELEASE, 2);

		assertThat(customer.statement(), is("Rental Record for Customer_name\n"
				+ "\t6.0(newReleaseMovieName)\n"
				+ "Amount owed is 6.0\n"
				+ "You earned 2 frequent renter pointers"));
	}

	@Test
	public void customerRentChildrensMovie4days(){
		createRental(CHILDRENS_MOVIE_NAME, Movie.CHILDRENS, 4);
		
		assertThat(customer.statement(), is("Rental Record for Customer_name\n"
				+ "\t3.0(childrensMovieName)\n"
				+ "Amount owed is 3.0\n"
				+ "You earned 1 frequent renter pointers"));
	}
}
````
- https://github.com/CODARI-JAMSIL/VideoRental