# miniProject_Calculator




using System;

namespace Calculator
{

    class Program
    {
        
        public static decimal downPayment;
        public static double interestRate;
        public static double loanTermMonths;
        public static decimal purchasePrice { get; private set; }
        

        static void Main()
        {

            while (true)
            {
                Console.WriteLine("What would you like to calculate? Choose option 1, 2 or 3: \n");

                Console.WriteLine("\t1. Basic Calculator");
                Console.WriteLine("\t2. Sales Tax Calculator");
                Console.WriteLine("\t3. Mortgage Calculator");

                int choice = Convert.ToInt32(Console.ReadLine());

                switch (choice)

                {
                    case 1:

                        BasicCalculator bc = new BasicCalculator();
                        Console.WriteLine("Basic Calculator \r");
                        Console.WriteLine("------------------------\n");

                            string numInput1 = "";
                            string numInput2 = "";
                            double result;

                            Console.WriteLine("Type a number, and then press Enter: ");
                            numInput1 = Console.ReadLine();

                            double cleanNum1 = 0;
                            while (!double.TryParse(numInput1, out cleanNum1))
                            {
                                Console.Write("This is not valid input. Please enter an integer value: ");
                                numInput1 = Console.ReadLine();
                            }

                            Console.Write("Type another number, and then press Enter: ");
                            numInput2 = Console.ReadLine();

                            double cleanNum2 = 0;
                            while (!double.TryParse(numInput2, out cleanNum2))
                            {
                                Console.Write("This is not valid input. Please enter an integer value: ");
                                numInput2 = Console.ReadLine();
                            }

                            Console.WriteLine("Choose an operator from the following list:");
                            Console.WriteLine("\ta - Add");
                            Console.WriteLine("\ts - Subtract");
                            Console.WriteLine("\tm - Multiply");
                            Console.WriteLine("\td - Divide: " + "Choosing 0 will result in math error");
                            Console.Write("\tp - Modulus");

                            string op = Console.ReadLine();

                            try
                            {
                                result = BasicCalculator.DoOperation(cleanNum1, cleanNum2, op);
                                if (double.IsNaN(result))
                                {
                                    Console.WriteLine("This operation will result in a mathematical error.\n");
                                }
                                else Console.WriteLine("Your result: {0:0.##}\n", result);
                            }
                            catch (Exception e)
                            {
                                Console.WriteLine("Oh no! An exception occurred trying to do the math.\n - Details: " + e.Message);
                            }
                            Console.WriteLine("------------------------\n");
                        break;


                    case 2:

                        Console.WriteLine("Tax Calculator \r");
                        Console.WriteLine("------------------------\n");
                        Console.Write("Enter Price of item: ");
                        string itemPrice = Console.ReadLine();
                        Console.Write("Enter tax rate (in percentage): ");
                        string taxRate = Console.ReadLine();

                        SalesTaxCalculator tc = new SalesTaxCalculator(itemPrice, taxRate);
                        tc.CalculateTotalPrice();
                        tc.GetTotalMsg();
                        Console.ReadLine();
                        Console.WriteLine("------------------------\n");
                        break;


                    case 3:

                        Console.WriteLine("Mortgage Calculator \r");
                        Console.WriteLine("-----------------------\n");

                        Console.Write("Enter the property price: ");
                        purchasePrice = Convert.ToDecimal(Console.ReadLine());

                        Console.Write("Enter the downpayment amount: ");
                        downPayment = Convert.ToDecimal(Console.ReadLine());

                        Console.Write("Enter the interest rate: ");
                        interestRate = Convert.ToDouble(Console.ReadLine());

                        Console.Write("Enter the loan term length/months: ");
                        loanTermMonths = Convert.ToDouble(Console.ReadLine());


                        MortgageCalculator mc = new MortgageCalculator(purchasePrice, downPayment, interestRate, loanTermMonths);
                        DisplayLoanInformation(mc);
                        mc.CalculatePayment();
              
                        void DisplayLoanInformation(MortgageCalculator mc)
                        {
                            Console.WriteLine("Purchase Price: {0:C}", mc.purchasePrice);
                            Console.WriteLine("Down Payment: {0:C}", mc.downPayment);
                            Console.WriteLine("Loan Amount: {0:C}", mc.LoanAmount);
                            Console.WriteLine("Annual Interest Rate: {0}%", mc.interestRate);
                            Console.WriteLine("Term: {0} years ({1} months)", mc.LoanTermYears, mc.loanTermMonths);
                            Console.WriteLine("Monthly Payment: {0:C}", mc.CalculatePayment());
                            Console.WriteLine();
                        }
                        Console.WriteLine("------------------------\n");
                        break;
                }
                continue;
            }
   
        }
           
    }

    public class BasicCalculator
        {
    
        public static double DoOperation(double num1, double num2, string op)
            {
                double result = double.NaN; 

                // witch statement to do the math.
                switch (op)
                {
                    case "a":
                        result = num1 + num2;
                        break;
                    case "s":
                        result = num1 - num2;
                        break;
                    case "m":
                        result = num1 * num2;
                        break;
                    case "d":
                        if (num2 != 0)
                        {
                            result = num1 / num2;
                        }
                        break;
                    case "p":
                        result = num1 % num2;
                        break;

                    // Return text for an incorrect option entry.
                    default:
                        break;
                }
                return result;
            }
        }

    public class SalesTaxCalculator
    {

        private static decimal itemPrice;
        private static decimal percentTaxRate;
        private static decimal totalPrice;
        private double itemPrice1;
        private double taxRate;


        public SalesTaxCalculator(string inputItemPrice, string inputTaxRate)
        {
            itemPrice = Decimal.Parse(inputItemPrice);
            percentTaxRate = Decimal.Parse(inputTaxRate) / 100;
        }

        public SalesTaxCalculator(double itemPrice1, double taxRate)
        {
            this.itemPrice1 = itemPrice1;
            this.taxRate = taxRate;
        }

        public void CalculateTotalPrice()
        {
            totalPrice = itemPrice + (itemPrice * percentTaxRate);
        }

        public void GetTotalMsg()
        {
            Console.WriteLine("The item price is {0:C} \n" +
                "The total price is {1:C} \n" +
                "The tax rate is {2:p}",
                itemPrice, totalPrice, percentTaxRate);
        }
    }

    public class MortgageCalculator
        {
            public const int MonthsPerYear = 12;
            public decimal purchasePrice;
            public decimal downPayment;
            public double interestRate;
            public double loanTermYears;
            public double loanTermMonths;

 
        public MortgageCalculator(decimal purchasePrice, decimal downPayment, double interestRate, double loanTermMonths)
        {
            this.purchasePrice = purchasePrice;
            this.downPayment = downPayment;
            this.interestRate = interestRate;
            this.loanTermMonths = loanTermMonths;
        }
     
        public decimal LoanAmount
            {
                get { return (purchasePrice - downPayment); }
            }

        public double LoanTermYears
            {
                get { return loanTermMonths / MonthsPerYear; }
                set { loanTermMonths = ((int)(value * MonthsPerYear)); }
            }

        public double EPSILON { get; private set; }

        public decimal CalculatePayment()
            {
                decimal payment = 0;

                if (loanTermMonths > 0)
                {
                    if (Math.Abs(interestRate) > EPSILON)
                    {
                        double rate = ((interestRate / MonthsPerYear) / 100);
                        double factor = (rate + (rate / (Math.Pow(rate + 1, loanTermMonths) - 1)));
                        payment = (LoanAmount * (decimal)factor);
                    }
                    else payment = (LoanAmount / (decimal)loanTermMonths);
                }
                return Math.Round(payment, 2);
            }

        }

}
