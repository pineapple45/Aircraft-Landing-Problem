#include <iostream>
#include <stdlib.h> //for system()
#include <windows.h> //for Sleep() and Beep()
#include <ctime>
using namespace std;
int main()
{
	time_t curr_time;
	curr_time = time(NULL);
//
	tm *tm_local = localtime(&curr_time);
//	cout << "Current local time : " << tm_local->tm_hour << ":" << tm_local->tm_min << ":" << tm_local->tm_sec;
//	return 0;

 int total_planes;
 float flight_gap, window_gap;

 string s ;
 cout<<"Enter total number of planes to be landed: ";
 cin>>total_planes;
 cout<<"Enter the min required landing gap in secs: ";
 cin>>flight_gap;
 cout<<"Enter the min required window gap in secs: ";
 cin>>window_gap;
 cout<<"Enter start to give the plane schedule for the day: ";
 cin>>s;


 if(s == "start" || s == "s")
 {
   float hour = tm_local->tm_hour;
   float minute =  tm_local->tm_min;
   float secs =  tm_local->tm_sec;

   float secs_early =  tm_local->tm_sec;
   float mins_early =  tm_local->tm_min;
   float hours_early =  tm_local->tm_hour;

   float secs_latest = tm_local->tm_sec;
   float mins_latest = tm_local->tm_min;
   float hours_latest = tm_local->tm_hour;

   float mainArray[total_planes][6];

   for(int i = 1;i<=total_planes;i++){
    time_t curr_time;
	curr_time = time(NULL);

	tm *tm_local = localtime(&curr_time);

	if(i == 1){
	cout<<i<<" number plane will be landing at: "<< hour << ":" << minute << ":" << secs;
	cout<<endl;
	cout<<endl;
	}
	else{
    secs_early =  secs_latest + flight_gap;
    mins_early = mins_latest;
    hours_early = hours_latest;

    secs_latest = secs_early + window_gap;

    if(secs_early >=60){
        secs_early = secs_early - 60;
        mins_early += 1;
    }
    if(mins_early >= 60){
        mins_early = mins_early - 60;
        hours_early += 1;
    }
    if(hours_early >= 24){
        hours_early = hours_early - 24;
    }

    if(secs_latest >=60){
        secs_latest = secs_latest - 60;
        mins_latest += 1;
    }
    if(mins_latest >= 60){
        mins_latest = mins_latest - 60;
        hours_latest += 1;
    }
    if(hours_latest >= 24){
        hours_latest = hours_latest - 24;
    }

    mainArray[i-1][0] = hours_early;
    mainArray[i-1][1] = mins_early;
    mainArray[i-1][2] = secs_early;
    mainArray[i-1][3] = hours_latest;
    mainArray[i-1][4] = mins_latest;
    mainArray[i-1][5] = secs_latest;
    cout<<i<<" number plane will have earliest landing window at: "<< hours_early << ":" << mins_early << ":" << secs_early<<" latest landing window at: "<< hours_latest << ":" << mins_latest << ":" << secs_latest;
	cout<<endl;
	cout<<endl;
	}
   }

   cout<<"The schedule time of plane 1 is booked at: "<< hour << ":" << minute << ":" << secs<<endl;

   for(int i = 1;i<total_planes;i++){
    cout<<"Enter the scheduled time of plane "<<i+1<<" in {hrs -> mins ->secs}: "<<endl;
    int hrs,minutes,secs;
    cin>>hrs>>minutes>>secs;
    if(hrs >= 0 || hrs <= 23){
      if(minutes >= 0 || minutes <= 59){
        if(secs >= 0 || secs <= 59){
            cout<<"The entered values are "<<hrs<<":"<<minutes<<":"<<secs<<endl;
        }
        else{
            cout<<"wrong seconds entered!! (Enter value in [0,60])";
        }
      }
      else{
        cout<<"wrong minutes entered!! (Enter value in [0,60])";
      }
    }
    else{
        cout<<"wrong hours entered!! (Enter value in [0,23])";
    }

    if(hrs >= mainArray[i][0] && hrs <= mainArray[i][3]){
//            cout<<hrs<<endl;
        if(minutes >= mainArray[i][1] && minutes <= mainArray[i][4]){
//            cout<<minutes<<endl;
            if(secs >= mainArray[i][2] && secs <= mainArray[i][5]){
//                cout<<secs<<endl;
                cout<<"Do u want to land the plane??(y/n)"<<endl;
                char ans;
                cin>>ans;
                if(ans == 'y'){

                cout<<"Plane landed at "<<hrs<<":"<<minutes<<":"<<secs<<endl;
                int delayed_secs = secs + 5;
                int delayed_minutes = minutes;
                int delayed_hrs = hrs;

                if(delayed_secs>=60){
                    delayed_secs = delayed_secs - 60;
                    delayed_minutes += 1;
                }
                if(delayed_minutes >= 60){
                    delayed_minutes = delayed_minutes - 60;
                    delayed_hrs += 1;
                }
                if(delayed_hrs >= 24){
                    delayed_hrs = delayed_hrs - 24;
                }
                cout<<"The landing gap will be from "<<hrs<<":"<<minutes<<":"<<secs<<" to "<<delayed_hrs<<":"<<delayed_minutes<<":"<<delayed_secs<<endl;

                int next_plane_hrs = mainArray[i+1][0];
                int next_plane_minutes = mainArray[i+1][1];
                int next_plane_Secs = mainArray[i+1][2]-5;

                if(next_plane_Secs<0){
                    next_plane_Secs = next_plane_Secs + 60;
                    next_plane_minutes -= 1;
                }
                if(next_plane_minutes < 0){
                    next_plane_minutes = next_plane_minutes + 60;
                    next_plane_hrs += 1;
                }
                if(next_plane_hrs < 0){
                    next_plane_hrs = next_plane_hrs + 24;
                }

                if(next_plane_hrs == delayed_hrs){
                    if(next_plane_minutes == delayed_minutes){
                            if(next_plane_Secs - delayed_secs > 0){
                                cout<<"The free window is open from "<<delayed_hrs<<":"<<delayed_minutes<<":"<<delayed_secs<<" to "<<next_plane_hrs<<":"<<next_plane_minutes<<":"<<next_plane_Secs<<endl;
                                cout<<"Do u want to use this window to land any airborne planes?(y/n)";
                                char ans;
                                cin>>ans;
                                if(ans == 'y'){
                                    cout<<"Airborne plane landed!!";
                                }
                                else if(ans == 'n'){
                                    cout<<" New airborne signal given out to overhead planes";
                                }

                        }
                    }
                }


                }
                else if(ans == 'n'){

                cout<<"Plane window missed from "<<mainArray[i][0]<<":"<<mainArray[i][1]<<":"<<mainArray[i][2]<<" to "<<mainArray[i][3]<<":"<<mainArray[i][4]<<":"<<mainArray[i][5]<<endl;
                cout<<"The vague landing gap will be from "<<mainArray[i][3]<<":"<<mainArray[i][4]<<":"<<mainArray[i][5]<<" to "<<mainArray[i+1][0]<<":"<<mainArray[i+1][1]<<":"<<mainArray[i+1][2]<<endl;
              }


                cout<<"The new landing schedule will be: "<<endl;

                for(int j = i+1;j<total_planes;j++){
                    cout<<j+1<<" number plane will have earliest landing window at: "<< mainArray[j][0] << ":" << mainArray[j][1] << ":" << mainArray[j][2]<<" latest landing window at: "<< mainArray[j][3] << ":" << mainArray[j][4] << ":" << mainArray[j][5]<<endl;
                }
                cout<<endl;

            }

        }
     }

     //loop ends here
    }

   }
 }


