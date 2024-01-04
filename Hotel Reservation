package hotel;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Scanner;

public class HotelManagementSystem {
	private static final String url = "jdbc:mysql://localhost:3306/hotel_db";
	private static final String userName = "root";
	private static final String password = "root";

	public static void main(String[] args) throws ClassNotFoundException, SQLException {
		try {
			Connection connection = DriverManager.getConnection(url, userName, password);
			System.out.println("Connected Sucessfully");
			

			while (true) {
				System.out.println();
				System.out.println("HOTEL MANAGEMEENT SYSTEM");
				Scanner scanner = new Scanner(System.in);
				System.out.println("1. Reserve a Room");
				System.out.println("2. View Reservation Room");
				System.out.println("3. Get Room No");
				System.out.println("4. Update Reservation");
				System.out.println("5. Deletion Room");
				System.out.println("0. Exit");
				int choice = scanner.nextInt();

				switch (choice) {
				case 1:
					reserveRoom(connection, scanner);
					break;

				case 2:
					viewReservation(connection);
					break;

				case 3:
					getRoomNumber(connection, scanner);
					break;

				case 4:
					updateReservation(connection, scanner);
					break;

				case 5:
					deleteReservatio(connection, scanner);
					break;

				case 0:
					exit();
					scanner.close();
					return;

				default:
					System.out.println("Invalid Choice Try Again");
				}

			}
		} catch (SQLException e) {
			e.printStackTrace();
		}catch(InterruptedException e) {
			throw new RuntimeException();
		}
	}

	private static void reserveRoom(Connection connection, Scanner scanner){
		try {
			System.out.println("Enter Guest Name: ");
			String guestName = scanner.next();
			scanner.nextLine();
			System.out.println("Enter Room Number: ");
			int roomNumber = scanner.nextInt();
			System.out.println("Enter Contact Number: ");
			String contactNumber = scanner.next();

			String sql = "INSERT INTO reservation (guest_name, room_no , contact_no)" + "VALUES ('" + guestName
					+ " ' , " + roomNumber + ", ' " + contactNumber + "')";
			try(Statement statement = connection.createStatement()){
				int afttectedRows = statement.executeUpdate(sql);

				if (afttectedRows > 0) {
					System.out.println("Execution Sucessful");
				} else {
					System.out.println("Execution failed");
				}
			}
		}
		catch (SQLException e) {
			e.printStackTrace();
		}
	}

	private static void viewReservation(Connection connection) throws SQLException {
		String sql =  "SELECT  reservation_id,guest_name, room_no, contact_no, reservation_date from reservation";
		try (Statement statement = connection.createStatement();
				ResultSet rs = statement.executeQuery(sql)) {
			while(rs.next()) {
			int reservatioId = rs.getInt("reservation_id");
			String guestName = rs.getString("guest_name");
			int roomNo = rs.getInt("room_no");
			String contactNo = rs.getString("contact_no");
			String reservationTime = rs.getTimestamp("reservation_date").toString(); 
			
			//Display the details in the table
			System.out.println(reservatioId+" " + guestName+ " " + roomNo+ " " + contactNo+ " " + reservationTime);
			}
		}
	}

	private static void getRoomNumber(Connection connection, Scanner scanner) {
		try {
		System.out.println("Enter Reservation Id : ");
		int reservationId = scanner.nextInt();
		scanner.nextLine();
		System.out.println("Enter guest Name : ");
		String guestName = scanner.nextLine();
		
		String sql = "SELECT room_no FROM reservation " +
                "WHERE reservation_id = " + reservationId +
                " AND guest_name = '" + guestName + "'";
		
		try(Statement statement = connection.createStatement();
				ResultSet rs = statement.executeQuery(sql)) {
			if(rs.next()) {
				int roomNumber = rs.getInt("room_no");
				System.out.println("Room Number for Reservation ID: " + reservationId + " and guest: " + guestName + " in room: " + roomNumber );
			}
			else {
				System.out.println("Reservation not found for the given ID and guest Name.");
			}
		}
	}
		catch(SQLException e) {
			e.printStackTrace();
			}
		}

	private static void updateReservation(Connection connection, Scanner scanner) {
		try {
			System.out.println("Enter Reservation ID to Update: ");
			int reservationId = scanner.nextInt();
			scanner.nextLine();

			if (!reservationExists(connection, reservationId)) {
				System.out.println("Rerservation not found for the given ID. ");
				return;
			}
			System.out.println("Enter New Guest Name: ");
			String newGuest = scanner.next();
			System.out.println("Enter New Room Number: ");
			int newRoom = scanner.nextInt();
			System.out.println("Enter New Contact Number: ");
			String newContact = scanner.next();

			String sql = "UPDATE reservation SET guest_name = '" + newGuest + "' , " 
			             + "room_no = " + newRoom + ", "
					     + "contact_no = '" + newContact + "'  " 
			             + "WHERE reservation_id = " + reservationId;
			try(Statement statement = connection.createStatement()) {
				int affectedRows = statement.executeUpdate(sql);
				
				if(affectedRows > 0) {
					System.out.println("Reservation Updated Sucessfully");
				}
				else {
					System.out.println("Rerservation Failed");
				}
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	private static void deleteReservatio(Connection connection, Scanner scanner) {
		try {
			System.out.println("Enter reservation ID to delete: ");
			int reservationId = scanner.nextInt();
			scanner.nextLine();
			if (!reservationExists(connection, reservationId)) {
				System.out.println("Rerservation not found for the given ID. ");
				return;
			}
			String sql = "DELETE FROM reservation WHERE reservation_id =" + reservationId;
			
			try (Statement statement = connection.createStatement()){
				int affectedRows = statement.executeUpdate(sql);
				
				if(affectedRows > 0) {
					System.out.println("Reservation deleted Sucessfully");
				}
				else {
					System.out.println("Deletion Failed");
				}
			}
		}catch(SQLException e) {
			e.printStackTrace();
		}
	}

	private static boolean reservationExists(Connection connection, int reservationId) {
		try {
			String sql = "SELECT reservation_id FROM reservation WHERE reservation_id = " + reservationId;
			
			try (Statement statement = connection.createStatement();
					ResultSet rs = statement.executeQuery(sql)){
				return rs.next();
		}
	}
	catch (SQLException e) {
		e.printStackTrace();
		return false;
		}
	}

	

	private static void exit() throws InterruptedException {
		System.out.println("Exiting System");
		int i = 5;
		while(i != 0) {
			System.out.println(".");
				Thread.sleep(450);
			i--;
		}
		System.out.println();
		System.out.println("Thank You Using Hotel Reservation System!!!");
	}

}
