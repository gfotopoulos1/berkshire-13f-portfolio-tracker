# berkshire-13f-portfolio-tracker
1. Project Overview & Objective
Το συγκεκριμένο μοντέλο αυτοματοποιεί την παρακολούθηση και σύγκριση των επενδυτικών θέσεων της Berkshire Hathaway μεταξύ δύο διαδοχικών τριμήνων. Μέσω ενός αυτοματοποιημένου pipeline στο Power Query, εξάγονται τα στοιχεία απευθείας από τα αρχεία XML της SEC, καθαρίζονται, ενοποιούνται και ταξινομούνται ανά είδος συναλλαγής.

2. Data Pipeline Architecture (ETL)
Q_Current (Staging): Άντληση XML τρέχοντος τριμήνου, κατάργηση περιττών μεταδεδομένων, μετατροπή τύπων δεδομένων και ομαδοποίηση (Group By) ανά CUSIP για άθροιση μετοχών/αξίας.
Q_Previous (Staging): Αντίστοιχη άντληση και καθαρισμός του αμέσως προηγούμενου τριμήνου.
Portfolio_Analysis (Production Table): Συνένωση των δύο συνόλων δεδομένων με Full Outer Join (JoinKind.FullOuter). Η μέθοδος αυτή εγγυάται μηδενική απώλεια δεδομένων τόσο για νέες εισόδους όσο και για πλήρεις ρευστοποιήσεις.

3. Business & Investment Logic
Η κατηγοριοποίηση κάθε θέσης στη στήλη Trade_Action υπολογίζεται αυτόματα με βάση τους εξής κανόνες:
NEW BUY: Προηγούμενες Μετοχές = 0 και Τρέχουσες Μετοχές > 0
SOLD OUT: Προηγούμενες Μετοχές > 0 και Τρέχουσες Μετοχές = 0
ADD: Μεταβολή Μετοχών (Δ" Shares") > 0
TRIM: Μεταβολή Μετοχών (Δ" Shares") < 0
UNCHANGED: Μεταβολή Μετοχών (Δ" Shares") = 0

4. How to Update for Next Quarter
Αντιγραφή νέων XML URLs από το SEC EDGAR.
Στο Excel: Data →Queries & Connections →δεξί κλικ στο query →Edit →αλλαγή URL στο βήμα Source.
Πατήστε Data →Refresh All.
![Uploading εικόνα.png…]()
