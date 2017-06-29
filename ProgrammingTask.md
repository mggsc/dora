Part 1 : String Generator 

    #include <iostream>
    #include <fstream> 
    #include <string>
    #include <time.h>
    #include <windows.h>  

    using namespace std;

    inline void setcursor(int x, int y)
    {
        HANDLE hCon = GetStdHandle(STD_OUTPUT_HANDLE);
        COORD setps;
        setps.X = x; setps.Y = y;
        SetConsoleCursorPosition(hCon, setps);
    }


    void GenerateRandomStr(long long capacity)
    {
        srand((unsigned)time(NULL));

        unsigned int lowDWORD = capacity & 0xFFFFFFFF;
        unsigned int highDWORD = (capacity>>32) & 0xFFFFFFFF;

        // Open the file that we want to map.
        HANDLE hFile = CreateFile(L"RandomStrings.txt", GENERIC_READ | GENERIC_WRITE,0,NULL,OPEN_ALWAYS,FILE_ATTRIBUTE_NORMAL,NULL);

        // Create a file-mapping object for the file.
        HANDLE hFileMapping = CreateFileMapping(hFile, NULL, PAGE_READWRITE, highDWORD, lowDWORD, NULL);

        char* pbFile = (char*)MapViewOfFile(hFileMapping, FILE_MAP_WRITE, 0, 0, 0);

        char* pAnsi = pbFile;
        double ratio = 0;
        int n = 0;
        int last_n = 0;
        int nMove = 0;
        char buf[10]={0};

        // display progress bar range
        printf("║");		// left
        setcursor(80 , 1);
        printf("║");		// right
        setcursor(2 , 1);   

        for(long long i = 0; i < capacity; i++)
        {
            if (rand() % 2 == 0)
                *pAnsi++ = 'a' + rand() % 26;
            else
                *pAnsi++ = 'A' + rand() % 26;


            // display progress bar on console
            ratio = (double)i / capacity * 100;

            n = 40 * i / capacity; // 40 bars to display
            if( n - last_n == 1)
            {
                printf("█"); 
                nMove+=2;	
            }

            //sprintf(buf, "%6.1f%%", ratio);
            //printf("%s", buf);
            //setcursor(2+nMove , 1);
            last_n = n;	
        }

        printf("\nCompleted!\n");

        UnmapViewOfFile(pbFile);
        CloseHandle(hFileMapping);
        CloseHandle(hFile);


       /*
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
        */
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
