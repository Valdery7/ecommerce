[MySQL Model (ecommerce).sql](https://github.com/user-attachments/files/22752084/MySQL.Model.ecommerce.sql)# ecommerce
Projeto Correio eletrônico

[Uploadin-- criação do banco de dados para o cenário de E-commerce
create database ecommerce;
use ecommerce;

create table clients(
	idClient int auto_increment primary key,
    Fname varchar (10),
    Minit char (3)
    Lname varchar (20),
    CPF char(11) not null,
    Address varchar (30)
    constraint unique_cpf_client unique (CPF)
    );

alter table clients auto_increment=1;

create table product(
	idProduct int auto_increment primary key,
    Pname varchar(10) not null,
    classification_kids bool default false,
    category enum('Eletrônico','Vestimenta','Brinquedos','Alimentos','Móveis'), not null,
    avaliação float default 0, 
    Size varchar(10),
    );

create table payments(
idclient int,
idPayment int,
typePayment enum ('Pix','Boleto','Cartão','Dois cartões'),
limitAvailable float,
dateValid
yearValid
primary key(idClient, id_Payment)
);

create table orders(
	idOrder int auto_increment primary key,
    idOrderClient int,
    orderStatus enum('cancelado','confirmado','Em processamento') default 'Em processamento',
    orderDescription varchar(255),
    sendValue float default 10,
    paymentCash bool default false,
    idPayment foreign key,
    constraint fk_orders_client foreign key (idOrderClient) references Clients(idClient)
    );

create table productStorage(
	idProdStorange int auto_increment primary key,
    storageLocation varchar(255)
    quantity int default 0
    );
    
create table supplier(
idSupplier int auto_increment primary key,
SocialName varchar(255) not null,
CNPJ char(15) not null,
contact char(11) not null,
constraint unique_supplier unique (CNPJ)
);

create table seller(
idSeller int auto_increment primary key,
SocialName varchar(255) not null,
AbstName varchar(255),
CNPJ char(15),
CPF char(9),
location varchar(255)
contact char(11) not null,
constraint unique_cnpj_seller unique (CNPJ),
constraint unique_cpf_seller unique (CPF)
);

create table productSeller(
idPseller int,
idPproduct int,
prodQuantity int default 1,
primary key (idPseller, idPproduct),
constraint fk_product_seller foreign key (idPseller) references seller (idSeller),
constraint fk_product_product foreign key (idPproduct) references product(idProduct)
);

create table productOrder(
idPOproduct int,
idPOorder int,
podQuantity int default 1,
poStatus enum('Disponível', 'Sem estoque') default 'Disponível',
primary key (idPOproduct, idPOorder),
constraint fk_productorder_seller foreign key (idPOproduct) references product (idProduct),
constraint fk_productorder_product foreign key (idPOorder) references orders(idOrder)
);

create table storageLocation(
idLproduct int,
idLstorage int,
location varchar(255) not null,
primary key (idLproduct, idLstorange),
constraint fk_storage_location_product foreign key (idLproduct) references product (idProduct),
constraint fk_storage_location_storage foreign key (idLstorange) references produtStorage(idProdStorage)
);

create table productSupplier(
idPsupplier int,
idPsProduct int,
quantity int not null,
primary key (idPsSupplier, idPsProduct),
constraint fk_product_supplier_supplier foreign key (idPsSupplier) references supplier(idSupplier),
constraint fk_supplier_product foreign key (idPsProduct) references product(idProduct)
);

show tables;

show databases;
use information_schema;
show tables;
desc referential_constraints;
select * from referential_constraints where constraint_schema = 'ecommerce';

g MySQL Model (ecommerce).sql…]()

[Queries_and_data_insertion_Valdery55.sql](https://github.com/user-attachments/files/22752066/Queries_and_data_insertion_Valdery55.sql)
-- inserção de dados e queries
use ecommerce;

show tables;
-- idClient, Fname, Lname, CPF, Address
insert into Clients (Fname, Minit, Lname, CPF, Address)
		values('Maria', 'M', 'Silva', 12346789,'rua silva de prata 29, Carangola - Cidade das flores'),
			('Matheus', '0', 'Pimentel', 987654321, 'rua alemeda 289, Centro - Cidade das flores'),
			('Ricardo', 'F', 'Silva', 45678913, 'avenida alemeda vinha 1009, Centro - Cidade das flores'),
            ('Julia', 'S', 'França', 789123456, 'rua lareijras 861, Centro - Cidade das flores'),
            ('Roberta', 'G', 'Assis', 98745631, 'avenidade koller 19, Centro - Cidade das flores'),
            ('Isabela', 'M', 'Cruz', 654789123, 'rua alemeda das flores 28, Centro - Cidade das flores'),
          
insert into product (Pname, classification_kids, category, avaliação, size) values
		('Fone de ouvido", false, 'Eletrônico','4, null), 
        ('Barbie Elsa', true, 'Brinquedos', '3',null),
        ('Body Carters', true, 'Vestimenta', '5' ,null),
        ('Microfone Vedo - Youtuber' ,False, 'Eletrônico', '4' ,null), 
        ('Sofá retrátil',False, 'Móveis', '3', '3x57×80'),
        ('Farinha de arroz' ,False, 'Alimentos', '2',null), 
        ('Fire Stick Amazon' , False, 'Eletrônico', '3',null);

select *from clients;
select *from product;

insert into orders (idOrderClient, orderStatus, orderDescripition, sendValue, paymentCash) values
							(1, default, 'compra via aplicativo', null, 1),
                            (2, default, 'compra via aplicativo', 50, 0),
                            (3, 'confirmado', null, null, 1),
                            (4, default, 'compra via web site', 150, 0);

select *from orders;
insert into productOrder (idPOproduct, idPOorder, poQuantity, poStatus) values
						(1,5,2 null),
                        (2,5,1, null),
                        (3,6,1 null);
                        
insert into productStorage (storageLocation, quantity) values
							('Rio de Janeiro',1000),
                            ('Rio de Janeiro',500),
                            ('São Paulo',10),
                            ('São Paulo',100),
                            ('São Paulo',10),
                            ('Brasília',60);
                            
insert into storageLocation (idLproduct, idLstorage, location) values
						(1,2,'RJ'),
                        (2,6,'GO');

Insert into supplier (SocialName, CNPJ, contact) values
		('Almeida e filhos', 123456789123456, '21985474'), 
		('Eletrônicos Silva',854519649143457, '21985484'),
		('Eletrônicos Valma', 934567893934695, '21975474');

select *from Supplier;
insert into productSupplier (idPsSupplier, idPsProduct, quantity) values
						(1,1,500),
                        (1,2,400),
                        (2,4,633),
                        (3,3,5),
                        (2,5,10);
                        
insert into seller (SocialName, AbstName, CNPJ, CPF, location, contact) values
						('Tech eletronics', null, 123456789456321, null, 'Rio de Janeiro', 219946287),
                        ('Botique Durgas', null, null,123456783, 'Rio de Janeiro', 219567895),
                        ('Kids World' ,null,456789123654485,null, 'São Paulo', 1198657484);
                        
select * from seller;
-- idPseller, idPproduct, prodQuantity
insert into productSeller (idPSeller, idPproduct, prodQuantity) values
						(1,6,80),
                        (2,7,10);
                        
select * from productSeller;

select count(*) from clients;
select * from clients c, orders o where c.idClient = idOrderClient;

select Fname, Lname, idOrder, orderStatus from clients c, orders o where c.idClient = idOrderClient;
select concat(Fname, ' ',Lname) as Client, idOrder as Request, orderStatus as Status from clients c, orders o where c.idClient = idOrderClient;

insert into orders (idOrderClient, orderStatus, orderDescripition, sendValue, paymentCash) values
							(2, default, 'compra via aplicativo', null, 1),
                            
select count(*) from clients c, orders o 
			where c.idClient = idOrderClient;

select * from clients c
					inner Join orders o ON c.idClient = o.IdOrderClient
					inner join productOrder p ON p.idPOorder = o.idOrder
					group by idClient;
			                
select c.idClient, Fname, count(*) as Number_of_orders from clients c 
					inner Join orders o ON c.idClient = o.IdOrderClient
					group by idClient;
                    
select * from clients order by Fname, Lname 

select c.idClient, Fname, count(*) as Number_of_orders 
					from clients c,
					group by idClient,
				    Order by Fname, Lname;

select * from clients c
					Group by Fname
                    Having Count (Fname) >10
					Order by Fname;
                    
HAVING COUNT(first_name) > 10
