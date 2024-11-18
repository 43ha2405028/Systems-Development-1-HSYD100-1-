public class WaterBillCalculator {
    
    // Constants for the thresholds and costs
    static final double SUMMER_THRESHOLD = 7000; // in litres
    static final double NON_SUMMER_THRESHOLD = 5000; // in litres
    static final double SUMMER_COST_PER_LITRE = 2.78; // in R
    static final double NON_SUMMER_COST_PER_LITRE = 2.65; // in R
    static final double SUMMER_EXTRA_COST_PER_LITRE = 0.62; // in R
    static final double NON_SUMMER_EXTRA_COST_PER_LITRE = 0.50; // in R
    static final double VAT_RATE = 0.15; // VAT rate
    static final double BASIC_PAYMENT = 38.00; // Basic payment in R
    static final double SUMMER_DISCOUNT = 0.12; // 12% discount in summer
    static final double NON_SUMMER_DISCOUNT = 0.20; // 20% discount outside summer
    
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Input the required data
        System.out.println("Enter the season (summer or non-summer): ");
        String season = scanner.nextLine().toLowerCase();

        System.out.println("Enter the water usage in litres for the month: ");
        double waterUsage = scanner.nextDouble();
        
        // Call method to calculate the bill
        double totalBill = calculateWaterBill(season, waterUsage);

        // Display the final detailed bill
        System.out.println("\nDetailed Water Bill:");
        System.out.printf("Basic Payment: R%.2f%n", BASIC_PAYMENT);
        System.out.printf("Water Usage: %.2f litres%n", waterUsage);
        System.out.printf("Total Bill Before Discount: R%.2f%n", totalBill);

        // Calculate the VAT and final amount
        double vat = totalBill * VAT_RATE;
        double finalBill = totalBill + vat;
        System.out.printf("VAT (15%%): R%.2f%n", vat);

        // Apply the discount based on season and threshold
        if (season.equals("summer")) {
            if (waterUsage <= SUMMER_THRESHOLD) {
                double discount = finalBill * SUMMER_DISCOUNT;
                finalBill -= discount;
                System.out.printf("Summer Discount (12%%): -R%.2f%n", discount);
            } else {
                double extraCharge = calculateExtraCharge(season, waterUsage, SUMMER_THRESHOLD);
                finalBill += extraCharge;
                System.out.printf("Extra Charge for Usage Above Threshold: R%.2f%n", extraCharge);
            }
        } else {
            if (waterUsage <= NON_SUMMER_THRESHOLD) {
                double discount = finalBill * NON_SUMMER_DISCOUNT;
                finalBill -= discount;
                System.out.printf("Non-Summer Discount (20%%): -R%.2f%n", discount);
            } else {
                double extraCharge = calculateExtraCharge(season, waterUsage, NON_SUMMER_THRESHOLD);
                finalBill += extraCharge;
                System.out.printf("Extra Charge for Usage Above Threshold: R%.2f%n", extraCharge);
            }
        }

        System.out.printf("Final Bill After Discount and VAT: R%.2f%n", finalBill);

        // Message indicating if the user is within or outside the threshold
        if ((season.equals("summer") && waterUsage <= SUMMER_THRESHOLD) || 
            (season.equals("non-summer") && waterUsage <= NON_SUMMER_THRESHOLD)) {
            System.out.println("You have used water within the threshold.");
        } else {
            System.out.println("You have used water outside the threshold.");
        }
        
        scanner.close();
    }

    // Method to calculate the total bill before VAT and discounts
    public static double calculateWaterBill(String season, double waterUsage) {
        double costPerLitre;
        double threshold;
        
        // Determine cost per litre and threshold based on the season
        if (season.equals("summer")) {
            costPerLitre = SUMMER_COST_PER_LITRE;
            threshold = SUMMER_THRESHOLD;
        } else {
            costPerLitre = NON_SUMMER_COST_PER_LITRE;
            threshold = NON_SUMMER_THRESHOLD;
        }
        
        // Calculate the initial cost
        double basicBill = BASIC_PAYMENT;
        double waterCost = waterUsage * costPerLitre;

        // Total bill before VAT and discount
        return basicBill + waterCost;
    }

    // Method to calculate extra charge for usage above threshold
    public static double calculateExtraCharge(String season, double waterUsage, double threshold) {
        double extraChargePerLitre;

        if (season.equals("summer")) {
            extraChargePerLitre = SUMMER_EXTRA_COST_PER_LITRE;
        } else {
            extraChargePerLitre = NON_SUMMER_EXTRA_COST_PER_LITRE;
        }

        // Calculate how much water exceeds the threshold and charge extra
        double excessWater = waterUsage - threshold;
        return excessWater * extraChargePerLitre;
    }
}