Part 1 : String Generator 

    #include <iostream>
    #include <fstream> 
    #include <string>
    #include <time.h>
    using namespace std;

    void getRandomStr(long long length)
    {
        ofstream outfile("data.txt", std::ios_base::app);
        srand((unsigned)time(NULL));
        string temp;

        for(long long i = 0; i<length; i++)
        {
            if (rand()%2==0)
                temp ='a'+rand()%26;
            else
                temp ='A'+rand()%26;
            outfile<<temp;

            float r = (float) i / length;
            cout<<"Completed: "<< r <<"%"<<endl;
        }

        outfile.close();
    }



    int main()
    {


        cout<<"Please input how many gigabytes of strings to generate:";
        int length = 0;
        cin>>length;  


        getRandomStr(length*1024*10);


        return 0;
    }
