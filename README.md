import sys
valid_groups = ['A+','A-','B+','B-','AB+','AB-','O+','O-']
Valid_Bgroup=  {
    "A+": ["A+", "A-", "O+", "O-"],
    "A-": ["A-", "O-"],
    "B+": ["B+", "B-", "O+", "O-"],
    "B-": ["B-", "O-"],
    "O+": ["O+", "O-"],
    "O-": ["O-"],
    "AB+": ["A+", "A-", "B+", "B-", "O+", "O-", "AB+", "AB-"],
    "AB-": ["A-", "B-", "O-", "AB-"]
}
Donor_Records = []
Request_records=[]
def get_data(field , valid_func = None):
    while True:
        try:
            donor_input= input("Enter donor's {}:".format(field)).strip()
            if not donor_input:
                print("Input invalid try again")
                continue
            if valid_func:
                return valid_func(donor_input)
            return donor_input
        except ValueError as error:
            print("Error:{error}")
        except Exception as exp:
            print("Unexpected Error:{exp}")


def check_bgroup(group=None):
    print(group)
    # valid_groups = ['A+','A-','B+','B-','AB+','AB-','O+','O-']
    uppergrp= group.upper()
    if uppergrp in valid_groups:
        return uppergrp
    else:
        raise ValueError ("'{group}' is not a valid blood group")
    
    
def check_age(age=0):
    try:
        print(age)
        age = int(age)
        if age<=0:
            print("Age must be in whole number")    
        elif 18<= age <= 65:
            return age
        else:
            raise ValueError("You are not qualified to donate blood")
    except ValueError:
        raise ValueError ("Age must be in whole number")
      


def add_donor():
    name=input("Enter your name:")
    blood_group = get_data("Blood Group(eg. O+ , A-)" , check_bgroup)
    age = get_data("Age" , check_age)
    phone = get_data("Phone No")
    city = get_data("City")

    new_donor = {'name':name,
                 'group':blood_group,
                 'age':age,
                 'phone':phone,
                  'city':city
    }
    Donor_Records.append(new_donor)
    print("New Donor Added Successfully")



def search_by_group():
    try:
         
        search_by_group= get_data("Blood Group to search", check_bgroup)
    except ValueError as e:
        print("Invalid Input")
        return
    
    acceptable_donor=Valid_Bgroup.get(search_by_group, [])
    found_donors=[]
    for donor in Donor_Records:
        donor_type=donor['group']
        if donor_type in acceptable_donor:
            found_donors.append(donor)
    if found_donors:
        print(f"\n Found {len(found_donors)} compatible donor(s)") 
        print(f"Compatible Donor Types:{','.join(acceptable_donor)}")
    else:
        print("No compatible donors currently registered")



def view_all_donors():
    if not Donor_Records:
        print("No donors are currently registered")
        return
    print("\n---Donor List---")
    for donor in Donor_Records:
        print(donor)


def submit_request():
    patient_name=get_data("Name")
    req_type=get_data("Requested Blood Type:",check_bgroup)
    units=get_data("Units Needed:")
    contacts=get_data("Contact Number:")

    new_request={'patient_name':patient_name,
                 'group':req_type,
                 'units':units,
                 'contacts':contacts
                

    }
    Request_records.append(new_request)
    print("Added successfully")




def view_requests():
    print(" All View Requests  ")
    if not Request_records:
        print("No active blood requests")
        return
    print("\n---  List ---")
    for req in Request_records:
        
        print(req) 
    

def main_menu():
    while True:
        print("\n----MAIN MENU----")
        print("1.Register new donor")
        print("2.View All Donors")
        print("3.Search for donors")
        print("4.Submit a blood request")
        print("5.View active requests")
        print("6.EXIT APPLICATION")
        

        choice = input("Enter choice(1-6): ").strip()

        if choice == "1":
            add_donor()

        elif choice == "2":
            view_all_donors()

        elif choice == "3":
            search_by_group()

        elif choice == "4":
            submit_request()

        elif choice == "5" :
            view_requests()

        elif choice == "6":
            print("Thank you!")
            break


        else:
            print("Invalid choice please choose again")



main_menu()
