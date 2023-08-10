string conString = ConfigurationManager.ConnectionStrings["DBCON"].ConnectionString;
string conString_live = ConfigurationManager.ConnectionStrings["DBCON_live"].ConnectionString;
 
 <!--- Select Query Get---->
 public DataTable GetDatatable(string querydata)
        {
            OracleCommand cmd2 = new OracleCommand(querydata);
            using (OracleConnection con = new OracleConnection(conString))
            {
                try
                {
                    if (con.State == ConnectionState.Open)
                    {
                        con.Close();
                    }
                    con.Open();
                    using (OracleDataAdapter sda = new OracleDataAdapter())
                    {
                        cmd2.Connection = con;

                        sda.SelectCommand = cmd2;
                        using (DataTable dt = new DataTable())
                        {
                            sda.Fill(dt);
                            return dt;
                        }
                    }
                }
                catch (Exception exrr)
                {
                    throw exrr;
                }
                finally
                {
                    con.Close();
                    // con.Dispose();
                }
            }
        }



      <!----Insert Query Execute--->
       public bool execquery(string query)
        {
            OracleTransaction query_trans;
            string conString = ConfigurationManager.ConnectionStrings["DBCON"].ConnectionString;
            using (OracleConnection con = new OracleConnection(conString))
            {
                OracleCommand cmd2 = new OracleCommand(query);
                try
                {
                    if (con.State == ConnectionState.Open)
                    {
                        con.Close();
                    }
                    con.Open();
                    cmd2.Connection = con;
                    query_trans = con.BeginTransaction(IsolationLevel.ReadCommitted);
                    int dr = cmd2.ExecuteNonQuery();
                    if (dr > 0)
                    {
                        query_trans.Commit();
                        return true;
                    }
                    else
                    {
                        query_trans.Commit();
                         return false;
                    }
                    //  return dr;
                }
                catch (Exception exrr)
                {
                    throw exrr;
                    //return false;
                }
                finally
                {
                    con.Close();
                    // con.Dispose();
                }
            }
        }

        <!---Get Single value---->
        public string GetQueryValue(string querydata)
        {
            string val = "";
            OracleCommand cmd2 = new OracleCommand(querydata);
            using (OracleConnection con = new OracleConnection(conString))
            {
                try
                {
                    if (con.State == ConnectionState.Open)
                    {
                        con.Close();
                    }
                    con.Open();
                    using (OracleDataAdapter sda = new OracleDataAdapter())
                    {
                        cmd2.Connection = con;

                        sda.SelectCommand = cmd2;

                        val = Convert.ToString(cmd2.ExecuteScalar());
                    }
                }
                catch (Exception exrr)
                {
                    throw exrr;
                }
                finally
                {
                    con.Close();
                    // con.Dispose();
                }
                return val;
            }
        }
