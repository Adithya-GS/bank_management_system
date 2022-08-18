# bank_management_system
import pymysql as db
import random
cnx = db.connect(host='localhost',user='root',password='adithya563@03',database='bank_management_system')
ptr = cnx.cursor()

def open_account():
    try:
        name = input("Enter Your Name : ")
        acc_no = generate()
        address = input("Enter Your Address : ")
        contact = int(input("Enter Your Contact Number : "))
        opening_bal = int(input("Enter The Opening Balance : "))
        sql1 = "insert into account values('{0}','{1}','{2}',{3},{4})".format(name,str(acc_no),address,contact,opening_bal)
        sql2 = "insert into amount values('{0}','{1}','{2}')".format(name,str(acc_no),opening_bal)
        ptr.execute(sql1)
        ptr.execute(sql2)
        cnx.commit()
        print(f"Your account has been successfull created and your account number is {acc_no}")
        main()
    except Exception as exp:
        print("Enter Valid Data")
        open_account()


def generate():
    acc_num = []
    num = random.randint(11111,99999)
    if num not in acc_num:
        acc_num.append(num)
        return num
    generate()


def deposit_amount():
    try:
        amount = int(input("Enter the amount to be deposited : "))
        ac_no = (input("Enter the account number : "),)
        sql3 = "select acc_no from amount "
        ptr.execute(sql3)
        account_numbers = ptr.fetchall()
        if ac_no in account_numbers:
            sql4 = "select balance from amount where acc_no = '{0}'".format(ac_no[0])
            ptr.execute(sql4)
            pre_amt = ptr.fetchone()
            amount = amount + pre_amt[0]
        else:
            print("-" * 40)
            print("Please Enter A Valid Account Number")
            print("-" * 40)
            deposit_amount()
        sql5 = "update amount set balance={0} where acc_no = '{1}'".format(amount, ac_no[0])
        ptr.execute(sql5)
        cnx.commit()
        print(f"Transaction Successfull, Available balance : {amount}")
        main()
    except Exception as e:
        print("-" * 30)
        print("Please Enter A Valid Data")
        print("-" * 30)
        deposit_amount()



def withdraw_amount():
    try:
        amount = int(input("Enter the amount to be withdrawn : "))
        ac_no = (input("Enter the account number : "),)
        sql3 = "select acc_no from amount "
        ptr.execute(sql3)
        account_numbers = ptr.fetchall()
        if ac_no in account_numbers:
            sql4 = "select balance from amount where acc_no = '{0}'".format(ac_no[0])
            ptr.execute(sql4)
            pre_amt = ptr.fetchone()
            amount = abs(amount - pre_amt[0])
        else:
            print("-" * 40)
            print("Please Enter A Valid Account Number")
            print("-" * 40)
            withdraw_amount()
        sql5 = "update amount set balance={0} where acc_no = '{1}'".format(amount, ac_no[0])
        ptr.execute(sql5)
        cnx.commit()
        print(f"Transaction Successfull, Available balance : {amount}")
        main()
    except Exception as e:
        print("-" * 30)
        print("Please Enter A Valid Data")
        print("-" * 30)
        deposit_amount()


def balance_enquiry():
    try:
        ac_no = (input("Enter the account number : "),)
        sql3 = "select acc_no from amount "
        ptr.execute(sql3)
        account_numbers = ptr.fetchall()
        if ac_no in account_numbers:
            sql4 = "select balance from amount where acc_no = '{0}'".format(ac_no[0])
            ptr.execute(sql4)
            pre_amt = ptr.fetchone()
            cnx.commit()
            print(f"Available Balance : {pre_amt[0]}")

        else:
            print("-" * 40)
            print("Please Enter A Valid Account Number")
            print("-" * 40)
            balance_enquiry()
        main()

    except Exception as e:
        print("-" * 30)
        print("Please Enter A Valid Data")
        print("-" * 30)
        balance_enquiry()


def display_details():
    try:
        ac_no = (input("Enter the account number : "),)
        sql3 = "select acc_no from amount "
        ptr.execute(sql3)
        account_numbers = ptr.fetchall()
        if ac_no in account_numbers:
            sql4 = "select * from account where acc_no = '{0}'".format(ac_no[0])
            ptr.execute(sql4)
            details = ptr.fetchone()
            sql5 = "select balance from amount where acc_no = '{0}'".format(ac_no[0])
            ptr.execute(sql5)
            amt = ptr.fetchone()
            cnx.commit()
            print("-"*20)
            print(f"Name : {details[0]}")
            print(f"Account Number : {details[1]}")
            print(f"Address : {details[2]}")
            print(f"Contact Number : {details[3]}")
            print(f"Available Balance : {amt[0]}")
            print("-" * 20)

        else:
            print("-" * 40)
            print("Please Enter A Valid Account Number")
            print("-" * 40)
            display_details()
        main()

    except Exception as e:
        print("-" * 30)
        print("Please Enter A Valid Data")
        print("-" * 30)
        display_details()


def close_account():
    try:
        ac_no = (input("Enter the account number : "),)
        sql3 = "select acc_no from amount "
        ptr.execute(sql3)
        account_numbers = ptr.fetchall()
        if ac_no in account_numbers:
            sql4 = "delete from amount where acc_no = '{0}'".format(ac_no[0])
            sql5 = "delete from account where acc_no = '{0}'".format(ac_no[0])
            ptr.execute(sql4)
            ptr.execute(sql5)
            cnx.commit()

        else:
            print("-" * 40)
            print("Please Enter A Valid Account Number")
            print("-" * 40)
            close_account()

    except Exception as e:
        print("-" * 30)
        print("Please Enter A Valid Data")
        print("-" * 30)
        close_account()


def Exit():
    cnx.close()
    exit()


def main():
    print('''
             1.Open New Account
             2.Deposit Amount
             3.Withdraw Amount
             4.Balance Enquiry
             5.Display Customer Details
             6.Close An Account
             7.Exit''')

    try:
        choice = int(input("Enter Your Choice : "))
        if choice == 1:
            open_account()
        elif choice == 2:
            deposit_amount()
        elif choice == 3:
            withdraw_amount()
        elif choice == 4:
            balance_enquiry()
        elif choice == 5:
            display_details()
        elif choice == 6:
            close_account()
        else:
            Exit()

    except Exception as e:
        print("-"*30)
        print("Please Enter A Valid Choice")
        print("-" * 30)
        main()


if __name__ == '__main__':
    main()
