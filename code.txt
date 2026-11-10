#include <stdio.h>

int main()
{
    int choice, count = 0, i;
    float amount, expenses[100], total;
    FILE *fp;

    // Load previous expenses
    fp = fopen("expenses.txt", "r");

    if(fp != NULL)
    {
        while(count < 100 &&
              fscanf(fp, "%f", &expenses[count]) == 1)
        {
            count++;
        }
        fclose(fp);
    }

    do
    {
        printf("\n===== STUDENT EXPENSE TRACKER =====\n");
        printf("1. Add Expense\n");
        printf("2. View Expenses\n");
        printf("3. Total Expenses\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch(choice)
        {
            case 1:
                if(count >= 100)
                {
                    printf("Expense limit reached!\n");
                    break;
                }

                printf("Enter expense amount: ");
                scanf("%f", &amount);

                if(amount > 0)
                {
                    fp = fopen("expenses.txt", "a");

                    if(fp == NULL)
                    {
                        printf("Unable to save expense!\n");
                        break;
                    }

                    expenses[count] = amount;
                    fprintf(fp, "%.2f\n", amount);
                    fclose(fp);

                    count++;
                    printf("Expense saved successfully!\n");
                }
                else
                {
                    printf("Enter a valid amount!\n");
                }
                break;

            case 2:
                if(count == 0)
                {
                    printf("No expenses found!\n");
                }
                else
                {
                    printf("\nYour Expenses:\n");

                    for(i = 0; i < count; i++)
                    {
                        printf("%.2f\n", expenses[i]);
                    }
                }
                break;

            case 3:
                total = 0;

                for(i = 0; i < count; i++)
                {
                    total += expenses[i];
                }

                printf("Total Expenses = %.2f\n", total);
                break;

            case 4:
                printf("Thank you for using the tracker!\n");
                break;

            default:
                printf("Invalid choice!\n");
        }

    } while(choice != 4);

    return 0;
}
