using System;
using System.Collections.Generic;

namespace HotelReservationSystem
{
    struct Reservation
    {
        public string GuestName;
        public int RoomNumber;
        public DateTime CheckInDate;
        public DateTime CheckOutDate;

        public void DisplayReservation()
        {
            Console.WriteLine($@"
            =============================================================
            =
            =
            =  Welcome to M.C.E's 5 Star Hotel                     
            =  Sta. Maria, Lal-lo, Cagayan                         
            =  REG.TIN: 123-456-0000000                            
            =                                                      
            =  Reserved!                                
            =                                                      
            =  Guest Name:      {GuestName.ToUpper()}               
            =  Room Number:     {RoomNumber}                        
            =  Check-In Date:   {CheckInDate.ToString("dd/MM/yyyy")}   
            =  Check-Out Date:  {CheckOutDate.ToString("dd/MM/yyyy")}
            =  
            =  
            =============================================================");
        }

        public void ProcessPayment()
        {
            double ratePerNight = 500.0;
            int numberOfNights = (CheckOutDate - CheckInDate).Days;
            double totalAmount = numberOfNights * ratePerNight;

            Console.WriteLine($"\nTotal Amount Due: Php {totalAmount}");
            Console.Write("Enter payment amount: Php  ");
            double payment = Convert.ToDouble(Console.ReadLine());
            double Change = payment - totalAmount;

            if (payment >= totalAmount)
            {
                Console.WriteLine($"Payment successful! Thank you for your payment.");
                Console.WriteLine();
                Console.WriteLine($"Change to be returned: Php {Change}");
                Console.WriteLine();
                Console.WriteLine("Generating receipt...");
                GenerateReceipt(totalAmount);
            }
            else
            {
                Console.WriteLine("Insufficient payment. Please try again.");
            }
        }

        private void GenerateReceipt(double totalAmount)
        {
            Console.WriteLine($@"
            =============================================================
            =                       RECEIPT                             
            =  Guest Name:      {GuestName.ToUpper()}               
            =  Room Number:     {RoomNumber}                        
            =  Total Amount:    Php {totalAmount}
            =  
            =  Payment Status:  PAID                                 
            =                                                      
            =============================================================");
        }
    }

    class Program
    {
        static List<Reservation> reservations = new List<Reservation>();

        static void Main(string[] args)
        {
            Console.WriteLine("Welcome to the Hotel Reservation System!");

            while (true)
            {
                Console.WriteLine("\n1. Make a Reservation");
                Console.WriteLine("2. Check Current Reservations");
                Console.WriteLine("3. Check-Out");
                Console.WriteLine("4. Exit");
                Console.Write("Select an option: ");
                string option = Console.ReadLine();

                switch (option)
                {
                    case "1":
                        MakeReservation();
                        break;
                    case "2":
                        CheckCurrentReservations();
                        break;
                    case "3":
                        CheckOut();
                        break;
                    case "4":
                        Console.WriteLine("Thank you for using the Hotel Reservation System!");
                        return;
                    default:
                        Console.WriteLine("Invalid option. Please try again.");
                        break;
                }
            }
        }

        static void MakeReservation()
        {
            Reservation reservation = new Reservation();

            Console.Write("Enter Guest Name: ");
            reservation.GuestName = Console.ReadLine();
            Console.Write("Enter Room Number: ");
            reservation.RoomNumber = Convert.ToInt32(Console.ReadLine());
            Console.Write("Enter Check-In Date ex:(dd-mm-yyyy): ");
            reservation.CheckInDate = DateTime.Parse(Console.ReadLine());
            Console.Write("Enter Check-Out Date ex:(dd-mm-yyyy): ");
            reservation.CheckOutDate = DateTime.Parse(Console.ReadLine());

            reservations.Add(reservation);
            reservation.DisplayReservation();
        }

        static void CheckCurrentReservations()
        {
            Console.WriteLine("\nCurrent Reservations:");
            foreach (var reservation in reservations)
            {
                reservation.DisplayReservation();
            }
        }

        static void CheckOut()
        {
            Console.Write("\nEnter guest name to check-out: ");
            string checkoutGuestName = Console.ReadLine();
            Reservation reservation = new Reservation();
            foreach (var r in reservations)
            {
                if (r.GuestName.ToLower() == checkoutGuestName.ToLower())
                {
                    reservation = r;
                    break;
                }
            }


            if (reservation.GuestName != null)
            {
                Console.WriteLine("Client name found. Proceeding with check-out and payment.");
                Console.WriteLine();
                reservation.ProcessPayment();
                reservations.Remove(reservation);
            }
            else
            {
                Console.WriteLine();
                Console.WriteLine("Guest name not found or does not match reservation. Client is likely still checked in or name is incorrect.");
            }
        }
    }
}
