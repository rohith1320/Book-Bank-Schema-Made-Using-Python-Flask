
from http import client
import mailbox
from multiprocessing.connection import Client
from urllib import response
from flask import Flask, redirect,render_template,request,url_for
import subprocess as sp
from flask.templating import render_template
from numpy import rec  
from pymongo import MongoClient
from mongopass import mongopass
import ssl
  
app=Flask("myapp")
server12=MongoClient(mongopass)
db=server12.bookbank
myCollection=db.userInfo
Myc1=db.books
orders=db.cart
test={"books":'Arnold',"Price":'199'}
Myc1.insert_one
xx=Myc1.find()

yy=orders.find()
AllBooks=[]
Price=[]
book=[]
address=[]
email=[]
for i in yy:
    book.append(i['book'])
    address.append(i['address'])
    email.append(i['email'])
for i in xx:
    AllBooks.append(i['books'])
    Price.append(i['Price'])


@app.route('/addtocart',methods=['GET','POST'])
def addtocart():
    if request.method=='POST':
        book_name=request.form['books']
        address=request.form['address']
        email=request.form['email']
        val={"book":book_name,"address":address,"email":email}
        # verify=myCollection.find_one({"email":val['email']})
        # verify2=Myc1.find_one({"books":val['book']})
        if (myCollection.find_one({"email":val['email']}) and myCollection.find_one({"email":val['email']})):
            orders.insert_one(val)
            return render_template("response.html",res="Your order was Placed Sucessfully")
        else:
            return render_template("response.html",res="Invalid Email id or Book Name")
    return render_template("addtocart.html")
@app.route('/home')
def home():    
    return render_template("home.html",len = len(AllBooks), Pokemons = AllBooks,Price=Price)
    
    #return render_template("home.html", len = len(Pokemons), Pokemons = Pokemons)
@app.route('/addbooks',methods=['GET','POST'])
def addbooks():
    
   
    
    if request.method=='POST':
        
        name=request.form['books']
        price=request.form['price']
        val={"books":name,"Price":price}
        
        
        result=Myc1.find_one({"books": val['books'] })
        if Myc1.find_one({"books": val['books'] }):
            return render_template("response.html",res="Book Already Present")
        else:
            Myc1.insert_one(val)
            return render_template("addbooks.html",len = len(AllBooks), Pokemons = AllBooks,Price=Price,book1=book,address1=address,email1=email,len1=len(book))
        
        
    return render_template("addbooks.html",len = len(AllBooks), Pokemons = AllBooks,Price=Price,book1=book,address1=address,email1=email,len1=len(book))
            

@app.route('/',methods=['GET', 'POST']) #decorator drfines the   
def register():
    if request.method=='POST':
        name=request.form['username']
        email=request.form['email']
        password=request.form['pass']
        
      
        myVal={"name":name,"email":email,"password":password}
        result=myCollection.find_one({"email": myVal['email'] })
        if myCollection.find_one({"email": myVal['email'] }):
            return render_template("response.html",res="Email Address Exist")
        else:
            y=myCollection.insert_one(myVal)
            return redirect(url_for('login'))
    return render_template("register.html")

    
@app.route('/login', methods=['GET', 'POST'])
def login():
    
    if request.method == 'POST':
        email=request.form['email']
        password1=request.form['pass']
        adminpass="admin12"
        myVal={"email":email,"password":password1}
        user=myCollection.find_one({"email": myVal['email'] })
        if(myCollection.find_one({"email":myVal['email']})):
              
        
            if(password1=='admin12'):
                return redirect(url_for('addbooks'))    
            if (user['password']==password1):
                mailid=user['email']
                user_name=user['name']
                
                return render_template("home.html",len = len(AllBooks), Pokemons = AllBooks,Price=Price,mailid1=mailid,user_name1=user_name)
                #return redirect(url_for('home'),mailid,user_name)
            else:
                return render_template("response.html",res="Invail Credentials")
        else:
            return render_template("response.html",res="Invalid Email Id")
            
    return render_template('login.html')
        


if __name__ =='__main__':  
    app.run(debug = True)
