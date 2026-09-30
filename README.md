# c-code-for-Fincalculator
fintech calculator
# Combined Finance Calculator

A modular, menu-driven C application engineered to simulate critical financial market investments. Built entirely using standard C libraries (`stdio.h`, `math.h`) to prioritize structural execution efficiency and safe, verified data validation pipelines.

## Project Track
* **Hackathon Track:** Hack Quest (AMS Codeathon 2026)
* **Developer:** Ganesh Sreevatsan T (First-Year CSE, AMSCE | IIT Madras BS Learner)

## Core Calculation Engines
1. **Lump-Sum / Compound Interest Engine:** Simulates long-term geometric compounding values mapped into sequential annual data outputs.
2. **SIP (Systematic Investment Plan) Future Target Value Engine:** Evaluates monthly recurring deposits computed via standard future annuity formulas.
3. **Multi-Frequency Fixed Deposit Module:** Supports toggling calculation logic across yearly, quarterly, and monthly compounding intervals.
4. **Mutual Fund Volatility Risk Analyzer:** Dynamically buckets structural asset risk profiles (<8% Low, 8-18% Moderate, >=18% High Risk) alongside worst-case market capital constraints.

## Technical Highlights
* **Defensive Pipeline Validation:** Integrated strict user console input buffer validation loops to intercept parsing exceptions, symbols, and negative variables, ensuring complete immunity to common terminal loop runtime lockups.
* **Domain Alignment:** Algorithmic calculation parameters are directly informed by standard regulatory guidelines from the National Stock Exchange (NSE) and SEBI consumer frameworks.
 
#include <stdio.h>
#include <math.h>

/* ---------- helper: safe float input with validation ---------- */
float getPositiveFloat(const char *prompt) {
    float value;
    while (1) {
        printf("%s", prompt);
        if (scanf("%f", &value) == 1 && value > 0) {
            return value;
        }
        printf("  Invalid input. Please enter a positive number.\n");
        while (getchar() != '\n'); /* clear bad input from buffer */
    }
}

int getPositiveInt(const char *prompt) {
    int value;
    while (1) {
        printf("%s", prompt);
        if (scanf("%d", &value) == 1 && value > 0) {
            return value;
        }
        printf("  Invalid input. Please enter a positive whole number.\n");
        while (getchar() != '\n');
    }
}

/* ---------- 1. Lump-sum / Compound Interest with year-by-year table ---------- */
void compoundInterestCalculator() {
    printf("\n===== Lump-Sum / Compound Interest Calculator =====\n");
    float principal = getPositiveFloat("Enter initial investment: Rs ");
    float rate = getPositiveFloat("Enter annual interest rate (in %%): ");
    int years = getPositiveInt("Enter number of years: ");

    float balance = principal;
    printf("\nYear-by-year growth:\n");
    printf("%-6s %-15s\n", "Year", "Balance (Rs)");
    for (int y = 1; y <= years; y++) {
        balance = balance * (1 + rate / 100.0);
        printf("%-6d %-15.2f\n", y, balance);
    }
    printf("\nAfter %d years, your investment grows from Rs %.2f to Rs %.2f\n",
           years, principal, balance);
    printf("Total gain: Rs %.2f\n", balance - principal);
}

/* ---------- 2. SIP Calculator ---------- */
void sipCalculator() {
    printf("\n===== SIP (Systematic Investment Plan) Calculator =====\n");
    float monthlyInvestment = getPositiveFloat("Enter monthly SIP amount: Rs ");
    float annualRate = getPositiveFloat("Enter expected annual return (in %%): ");
    int years = getPositiveInt("Enter investment duration (years): ");

    int months = years * 12;
    float monthlyRate = annualRate / 12.0 / 100.0;
    float futureValue = monthlyInvestment *
        (((pow(1 + monthlyRate, months) - 1) / monthlyRate) * (1 + monthlyRate));

    float totalInvested = monthlyInvestment * months;
    float totalGain = futureValue - totalInvested;

    printf("\nTotal invested over %d years: Rs %.2f\n", years, totalInvested);
    printf("Estimated maturity value: Rs %.2f\n", futureValue);
    printf("Estimated wealth gained: Rs %.2f\n", totalGain);
}

/* ---------- 3. Fixed Deposit Calculator ---------- */
void fixedDepositCalculator() {
    printf("\n===== Fixed Deposit (FD) Calculator =====\n");
    float principal = getPositiveFloat("Enter deposit amount: Rs ");
    float rate = getPositiveFloat("Enter annual interest rate (in %%): ");
    int years = getPositiveInt("Enter tenure (years): ");
    int compoundingPerYear = getPositiveInt("Compounding frequency per year (1=yearly, 4=quarterly, 12=monthly): ");

    float n = (float)compoundingPerYear;
    float maturity = principal * pow(1 + (rate / 100.0) / n, n * years);

    printf("\nMaturity amount after %d years: Rs %.2f\n", years, maturity);
    printf("Interest earned: Rs %.2f\n", maturity - principal);
}

/* ---------- 4. Mutual Fund Risk & Growth Analyzer ---------- */
void mutualFundAnalyzer() {
    printf("\n===== Mutual Fund Risk & Growth Analyzer =====\n");
    float investment = getPositiveFloat("Enter investment amount: Rs ");
    float expectedReturn = getPositiveFloat("Enter expected annual return (in %%): ");
    float volatility = getPositiveFloat("Enter expected annual volatility / std deviation (in %%): ");
    int years = getPositiveInt("Enter investment horizon (years): ");

    /* Simple risk classification based on volatility */
    const char *riskCategory;
    if (volatility < 8.0) {
        riskCategory = "LOW RISK (Debt-oriented / Conservative fund)";
    } else if (volatility < 18.0) {
        riskCategory = "MODERATE RISK (Balanced / Hybrid fund)";
    } else {
        riskCategory = "HIGH RISK (Equity-oriented / Aggressive fund)";
    }

    float expectedValue = investment * pow(1 + expectedReturn / 100.0, years);
    float bestCase = investment * pow(1 + (expectedReturn + volatility) / 100.0, years);
    float worstCase = investment * pow(1 + (expectedReturn - volatility) / 100.0, years);
    if (worstCase < 0) worstCase = 0;

    printf("\nRisk Category: %s\n", riskCategory);
    printf("Expected value after %d years: Rs %.2f\n", years, expectedValue);
    printf("Best-case estimate (return + volatility): Rs %.2f\n", bestCase);
    printf("Worst-case estimate (return - volatility): Rs %.2f\n", worstCase);
}

/* ---------- Main menu ---------- */
int main() {
    int choice;

    do {
        printf("\n======================================\n");
        printf("      COMBINED FINANCE CALCULATOR\n");
        printf("======================================\n");
        printf("1. Lump-Sum / Compound Interest Calculator\n");
        printf("2. SIP Calculator\n");
        printf("3. Fixed Deposit (FD) Calculator\n");
        printf("4. Mutual Fund Risk & Growth Analyzer\n");
        printf("5. Exit\n");
        printf("Choose an option (1-5): ");

        if (scanf("%d", &choice) != 1) {
            printf("Invalid input. Please enter a number.\n");
            while (getchar() != '\n');
            continue;
        }

        switch (choice) {
            case 1: compoundInterestCalculator(); break;
            case 2: sipCalculator(); break;
            case 3: fixedDepositCalculator(); break;
            case 4: mutualFundAnalyzer(); break;
            case 5: printf("\nThank you for using the Finance Calculator!\n"); break;
            default: printf("Invalid choice. Please select 1-5.\n");
        }

    } while (choice != 5);

    return 0;
}