package com;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.HashMap;

public class CustomerProductDao {
	PreparedStatement ps=null;
	Connection con=null;
	ResultSet rs=null;
public boolean addCustomerAndProduct(Customer customer)
{
	boolean flag=false;
	
	try
	{
		con=DBUtil.getConnection();
		String sql=("insert into TBL_Customer_1334368 values(seq_1.nextval,?,?,?)");
		ps=con.prepareStatement(sql);
		ps.setString(1,customer.getCustomerName());
		ps.setString(2, customer.getAddress());
		ps.setLong(3,customer.getMobile());
		int t=ps.executeUpdate();
		if(t>0)
		{
			/*for(Product p:customer.getP())
			{
				String sql1=("insert into TBL_Product_1334368 values(Productid_seq_1.nextval,?,?,?,?");
				ps=con.prepareStatement(sql1);
				ps.setInt(1,p.getProductid());
				ps.setString(2, p.getProductName());
				ps.setString(3, p.getProductType());
				ps.setInt(4, p.getPrice());
				ps.setInt(5, p.getQuantity());
				ps.executeUpdate();
				flag=true;
			}*/
			flag=true;
		}
	}
	catch(SQLException e){System.out.println(e.getMessage());}
	finally{DBUtil.closeAllConnection(con, ps, rs);}
	return flag;
	}
public ArrayList<Product> getProductbyCustomer(int Customerid)
{
	ArrayList<Product> pr=new ArrayList<Product>();
	try
	{
		con=DBUtil.getConnection();
		String sql=("select * from TBL_Product_1334368 p inner join TBL_Customer_1334368 c on p.Customerid=c.Customerid where p.Customerid=? order by Productid DESC ");
		ps=con.prepareStatement(sql);
		ps.setInt(1, Customerid);
		rs=ps.executeQuery();
		while(rs.next())
		{
			Product p=new Product(rs.getInt(1), rs.getString(2), rs.getString(3),rs.getInt(4),rs.getInt(5),rs.getInt(6));
			pr.add(p);
		}
		DBUtil.closeAllConnection(con, ps, rs);
		
	}
	catch(Exception e)
	{
		
	}
	return pr;
}
public HashMap<Integer,Integer> getProductCountPerCustomer()
{
	HashMap<Integer,Integer> HM=new HashMap<Integer,Integer>();
	try
	{
	con=DBUtil.getConnection();
	String str=("select customerid,count(*) as count from TBL_Product_1334368 p inner join TBL_Customer_1334368 c on p.Customerid=c.Customerid group by Customerid ");
	ps=con.prepareStatement(str);
	rs=ps.executeQuery();
	while(rs.next())
	{
		HM.put(rs.getInt("Customerid"), rs.getInt("count"));
	}
	DBUtil.closeAllConnection(con, ps, rs);
	}
	catch(Exception e)
	{
		
	}
	return HM;
}
public boolean deleteCustomer(int Customerid)
{
	boolean flag =false;
	int check=0;
	try
	{
		con=DBUtil.getConnection();
		String sql=("delete from TBL_Product_1334368 where Customerid=?");
		ps=con.prepareStatement(sql);
		ps.setInt(1, Customerid);
		check=ps.executeUpdate();
		 sql=("delete from TBL_Customer_1334368 where Customerid=?");
			ps=con.prepareStatement(sql);
			ps.setInt(1, Customerid);
			ps.executeUpdate();
			flag=true;
		
		DBUtil.closeAllConnection(con, ps, rs);
		
	}
	catch(Exception e)
	{
		
	}
	return flag;
}
public Customer getCustomerDetails(int productid)
{
	Customer c=null;
	try
	{
		con=DBUtil.getConnection();
		String sql=("Select * from TBL_Customer_1334368 c inner join TBL_Product_1334368 p  on c.Customerid=p.Customerid where p.Productid=?");
		ps=con.prepareStatement(sql);
		ps.setInt(1,productid);
		rs=ps.executeQuery();
		while(rs.next())
		{
			c=new Customer(rs.getInt(1), rs.getString(2), rs.getString(3), rs.getLong(4));
		}
		DBUtil.closeAllConnection(con, ps, rs);
	}
	catch(Exception e)
	{
		
	}
	return c;
}
public boolean updateProductQuantityAndPrice(int productId, int quantity, int price)
{
boolean flag=false;
int check=0;
try{
	con=DBUtil.getConnection();
	String str=("update TBL_Product_1334368 set Quantity=? and set price=? where Productid=?");
	ps=con.prepareStatement(str);
	ps.setInt(1, quantity);
	ps.setInt(2, price);
	ps.setInt(3, productId);
	check=ps.executeUpdate();
	if(check>0)
	{
		flag=true;
	}
	DBUtil.closeAllConnection(con, ps, rs);
}
catch(Exception e)
{
	
}
return flag;
}
public void getCountByType()
{
	try
	{
		con=DBUtil.getConnection();
		String str=("select ProductType,count(*) as count from TBL_Product_1334368 group by ProductType");
		ps=con.prepareStatement(str);
		rs=ps.executeQuery();
		while(rs.next())
		{
			System.out.println(rs.getString("ProductType")+" "+rs.getInt("count"));
		}
		DBUtil.closeAllConnection(con, ps, rs);
		
	}
	catch(Exception e)
	{
		
	}
}
public Customer getCustomerAndProduct(int customerId)
{
	Customer c=null;
	
	ArrayList<Product> pp= new ArrayList<Product>();
try
{
	con=DBUtil.getConnection();
	String str=("Select * from TBL_Customer_1334368 c inner join TBL_Product_1334368 p on c.Customerid=p.Customerid where c.Customerid=?");
	ps=con.prepareStatement(str);
	ps.setInt(1, customerId);

	rs=ps.executeQuery();
	while(rs.next())
	{
		c=new Customer(rs.getInt(1), rs.getString(2), rs.getString(3), rs.getLong(4));
		if(c!=null)
		{
			Product p=new Product(rs.getInt(1), rs.getString(2), rs.getString(3),rs.getInt(4),rs.getInt(5),rs.getInt(6));
			
			pp.add(p);
		}
		c.setP(pp);
	}
	DBUtil.closeAllConnection(con, ps, rs);
}
catch(Exception e)
{
	
}
return c;
}
public ArrayList<Customer> viewDetails()
{
	ArrayList<Customer> c=new ArrayList<Customer>();
	try
	{
		con=DBUtil.getConnection();
		String str=("Select * from TBL_Customer_1334368");
		ps=con.prepareStatement(str);
		rs=ps.executeQuery();
		while(rs.next())
		{
			Customer cc=new Customer(rs.getInt(1), rs.getString(2), rs.getString(3), rs.getLong(4));
			c.add(cc);
			
		}
		DBUtil.closeAllConnection(con, ps, rs);
		
	}
	catch(Exception e)
	{
		
	}
	
	
	
	return c;
	
}}















