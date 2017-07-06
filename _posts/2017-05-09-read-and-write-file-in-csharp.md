---
title: "Read and Write Files in C#"
date: 2017-05-09
category: C#
tags: [basic, theory, CSharp]

---

The operation of writing and reading file is very common in programming. Today I will introduce some fixed steps for the file operation in C# which can help us do this kind of operation more stable and make less mistake.

### Five steps for writing a file:    
1. Create file stream  

        FileStream fs = new FileStream("c:\\filename.filename.txt", FileMode.Create);
          
2. Create stream writter  
        
        StreamWriter sw = new StreamWriter(fs, Encoding.Unicode);
          
3. Write the data into a file by the stream way  

        foreach (string item in objPlaylist.ReadPathsFromList())  
        {  
            sw.WriteLine(item);  
        }
                
4. Close stream writter  
    
        sw.Close();
          
          
5. Close file stream
  
        fs.Close();
          
          
- - - 

### Five steps for reading a file:  
1. Create file stream  

        FileStream fs = new FileStream("c:\\filename\\filename.txt", FileMode.Open);
          
2. Create stream reader  
        
        StreamReader sr = new StreamReader(fs, Encoding.Unicode);
          
3. Read the data from a file by the stream way  

        string item = "";
        while ((item = sr.ReadLine()) != null)
        {
            this.lbxPlaylist.Items.Add(this.playlist.AddPathToList((string)item));
        }
                
4. Close stream reader  
    
        sr.Close();
          
          
5. Close file stream
  
        fs.Close();
          

- - -

Now I will give an example about how to write a log function  

        FileStream fs = new FileStream("", FileMode.Append);
        StreamWriter sw = new StreamWriter();
        sw.WriteLine(DateTime.Now.ToString() + "[The file operation is good!]");
        sw.Close();
        fs.Close();
        




