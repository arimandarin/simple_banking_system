import random
import sqlite3

class Banking_System:

    def __init__(self):
        self.conn = sqlite3.connect(
            'C:\\Users\\berin\\Documents\\programming\\python\\JetBrains\\banking_system\\card.sqlite')
        self.cur = self.conn.cursor()
        self.create_table()

    def create_table(self):
        self.cur.execute('''
        CREATE TABLE IF NOT EXISTS card(
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        number TEXT UNIQUE,
        pin TEXT,
        balance INTEGER DEFAULT 0 NOT NULL
        )
        ''')
        self.conn.commit()

    def account(self):
        print("1. Create an account\n2. Log into account\n0. Exit")
        try:
            a = int(input())
        except ValueError:
            print("You should enter numbers")
            account()
        if a == 1:
            self.create()
        elif a == 2:
            self.log()
        elif a == 0:
            self.out()
        else:
            print("Unknown option")
            self.account()

    def create(self):
        print("Your card has been created")
        print("Your card number:")
        self.luhn_alg()
        print("Your card PIN:")
        pin = random.randint(1000, 9999)
        print(pin)
        self.cur.execute('INSERT INTO card (number, pin) VALUES (?,?)', (self.card_number, pin))
        self.conn.commit()
        self.account()
    
    def luhn_alg(self):
        bank_identifier = 400000
        account_identifier = random.randint(100000000, 999999999)
        self.card_number = str(bank_identifier) + str(account_identifier)
        lista = list(self.card_number)
        step1 = [int(lista[x])*2 if x % 2 == 0 else int(lista[x]) for x in range(len(lista))]
        step2 = [step1[x]-9 if step1[x] > 9 else step1[x] for x in range(len(step1))]
        step3 = sum(step2)
        checksum = "0"
        if step3 % 10 == 0:
            self.card_number += checksum
        else:
            checksum = str(10 - (step3 % 10))
            self.card_number += checksum
        n = int(self.card_number)
        print(n)
        
    def log(self):
        while True:
            print("Enter your card number:")
            self.card = int(input())
            print("Enter your PIN number:")
            pin = int(input())
            self.cur.execute('SELECT number, pin FROM card WHERE number=? AND pin=?', (self.card, pin))
            sel = self.cur.fetchone()
            if sel:
                print("You have successfully logged in!")
                self.conn.commit()
                self.inside()
            else:
                print("Wrong card number or PIN!")
                self.account()
            break

    def inside(self):
        print("1. Balance\n2. Add income\n3. Do transfer\n4. Close account\n5. Log out\n0. Exit")
        a = int(input())
        if a == 1:
            self.balance()
        elif a == 2:
            self.income()
        elif a == 3:
            print("Transfer")
            print("Enter card number: ")
            transfer_card = int(input())
            self.transfer(transfer_card)
        elif a == 4:
            self.cur.execute('DELETE FROM card WHERE number=?', (self.card,))
            self.conn.commit()
            print("The account has been closed!")
            self.account()
        elif a == 5:
            print("You have successfully logged out!")
            self.account()
        elif a == 0:
            self.out()

    def income(self):
        print("Enter income: ")
        inc = int(input())
        self.cur.execute('UPDATE card SET balance=balance+? WHERE number =?', (inc, self.card))
        self.conn.commit()
        print("Income added")
        self.inside()

    def balance(self):
        self.cur.execute('SELECT balance FROM card WHERE number=?', (self.card, ))
        sel = self.cur.fetchone()
        for i in sel:
            print("Balance:", i)
        self.conn.commit()
        self.inside()

    def transfer(self, destination_number):
        if destination_number != self.card:
            if self.check_luhn(destination_number) is True:
                if self.check_card_db(destination_number) is True:
                    print("Enter how much money you want to transfer: ")
                    money = int(input())
                    self.cur.execute('SELECT balance FROM card WHERE number=?', (self.card,))
                    mon = self.cur.fetchone()
                    for i in mon:
                        if i >= money:
                            self.cur.execute('UPDATE card SET balance=balance-? WHERE number =?', (money, self.card))
                            self.cur.execute('UPDATE card SET balance=balance+? WHERE number =?', (money, destination_number))
                            self.conn.commit()
                            print("Success!")
                            self.inside()
                        else:
                            print("Not enough money!")
                            self.inside()
                else:
                    print("Such a card does not exist.")
                    self.inside()
            else:
                print("Probably you made mistake in the card number. Please try again!")
                self.inside()
        else:
            print("You can't transfer money to the same account!")
            self.inside()

    def check_card_db(self, number):
        self.cur.execute("SELECT number FROM card WHERE number=?", (number,))
        if self.cur.fetchone() is not None:
            self.conn.commit()
            return True

    def check_luhn(self, number):
        check = str(number)[:15]
        checksum = str(number)[-1]
        lista = list(check)
        step1 = [int(lista[x])*2 if x % 2 == 0 else int(lista[x]) for x in range(len(lista))]
        step2 = [step1[x]-9 if step1[x] > 9 else step1[x] for x in range(len(step1))]
        step3 = sum(step2)
        return ((step3 + int(checksum)) % 10) == 0

    def out(self):
        print("Bye!")
        self.conn.commit()
        self.cur.close()
        self.conn.close()
        quit()


bank = Banking_System()
print(bank.account())
