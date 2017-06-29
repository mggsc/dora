Part 1 : String Generator 

    #include <iostream>
    #include <fstream> 
    #include <string>
    #include <time.h>
    #include <windows.h>  

    using namespace std;

    void setcursor(int x, int y)
    {
        HANDLE hCon = GetStdHandle(STD_OUTPUT_HANDLE);
        COORD setps;
        setps.X = x; setps.Y = y;
        SetConsoleCursorPosition(hCon, setps);
    }



    void GenerateRandomStr(long long capacity)
    {
        ofstream outfile("data.txt", std::ios_base::app);

        srand((unsigned)time(NULL));

        string temp;
        char buf[10]={0};

        double ratio = 0;
        int n = 0;
        int last_n = 0;
        int nMove = 0;

        printf("║");

        for(long long i = 0; i < capacity; i++)
        {
            if (rand() % 2 == 0)
                temp = 'a' + rand() % 26;
            else
                temp = 'A' + rand() % 26;

            outfile << temp;

            ratio = (double)i / capacity * 100;

            n = 40 * i / capacity; // 40 bars to display
            if( n - last_n == 1)
            {
                printf("█"); 
                nMove+=2;	
            }


            sprintf(buf, "%6.1f%%", ratio);
            printf("%s", buf);
            setcursor(2+nMove , 1);

            last_n = n;	
        }
        printf("\nCompleted!\n");

        outfile.close();
    }



    int main()
    {
        unsigned int amount = 0;
        printf("Input the amount of gigabytes of strings to generate:");
        scanf("%d", &amount);  

        long long capacity =  amount << 20; // Mb to byte

        // hide cursor
        HANDLE hOut = GetStdHandle(STD_OUTPUT_HANDLE);
        CONSOLE_CURSOR_INFO cci;
        GetConsoleCursorInfo(hOut,&cci);
        cci.bVisible=false;
        SetConsoleCursorInfo(hOut,&cci);

        GenerateRandomStr(capacity);

        getchar();
        return 0;
    }
