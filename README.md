# LogIP
<code>
package responsibility;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.text.*;
import java.util.*;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Solution {
	static String pathToFile = "C:\\Users\\zhakypbekov_30855\\Desktop\\test1\\example.txt";
	private static List<String> list = new ArrayList<String>();
	private static final String IPADDRESS_PATTERN =   
			"^([01]?\\d\\d?|2[0-4]\\d|25[0-5])\\." +  
			"([01]?\\d\\d?|2[0-4]\\d|25[0-5])\\." +  
			"([01]?\\d\\d?|2[0-4]\\d|25[0-5])\\." +  
			"([01]?\\d\\d?|2[0-4]\\d|25[0-5])";  
	private static final String DATE_PATTERN =  "([0-3][0-9].)" + "([0]?\\d?|1[0-2])\\." + "([2-3]\\d\\d\\d)" + " " + "([0-2]?\\d):" + "([0-6]?\\d):" + "([0-6]?\\d)";
	private static Pattern pattern = Pattern.compile(DATE_PATTERN);
	private static Pattern ipPattern = Pattern.compile(IPADDRESS_PATTERN);
	private static DateFormat format = new SimpleDateFormat("dd.MM.yyyy HH:mm:ss");
	
	
	public static void main(String[] args) throws IOException, ParseException {
				
		Date d =  format.parse("11.12.2013 10:11:12");
		Date d2 = format.parse("11.12.2013 10:11:12"); 
		Date d3 = format.parse("14.11.2015 07:08:02"); 

		for(String s : getIPsForStatus(Status.FAILED,d,d2)){
			System.out.println(s);
		}
		
    }

     public static int getNumberOfUniqueIPs(Date after, Date before) throws IOException, ParseException{
 		 return getUniqueIPs(after, before).size();
     }
     // Должен возвращать множество, содержащее все не повторяющиеся IP адреса за выбранный период.
     
     public static Set<String> getUniqueIPs(Date after, Date before)  throws IOException, ParseException{
    	 Set<String> set = new HashSet<String>();
    	 addToList();
    	 set.addAll(IPslist(after,before));
 		 return set;
     }
     
     // Должен возвращать IP адреса, с которых работал переданный пользователь за выбранный период.
     
     public static Set<String> getIPsForUser(String user, Date after, Date before) throws IOException, ParseException{
    	 Set<String> uniquIPs = getUniqueIPs(after, before);
    	 Set<String> IPsForUser = new HashSet<String>();
    	 for(String str : list){
    		 if(str.indexOf(user) > 0){
    			 Matcher matcher = ipPattern.matcher(str);
    			 if(matcher.find()){
    				 String ip = matcher.group();
    				 if(uniquIPs.contains(ip)){
    					 IPsForUser.add(ip);
    				 }
    			 }
    		 }
    	 }
    	 return IPsForUser;
     }
     
     // Должен возвращать IP адреса, с которых было произведено переданное событие за выбранный период.     
     
     public static Set<String> getIPsForEvent(Event event, Date after, Date before) throws IOException, ParseException{
    	 Set<String> IPsForEvent = new HashSet<String>();
    	 List<String> IPslist =  IPslist(after,before);
    	 for(String str : list){
    		 if(str.indexOf(event.toString()) > 0){
    			 Matcher matcher = ipPattern.matcher(str);
    			 if(matcher.find()){
    				 String IP = matcher.group();
    				 if(IPslist.contains(IP))
    					 IPsForEvent.add(IP);
    			 }
    		 }
    	 }
    	 return IPsForEvent;
     }
     
     
  // Возвращает Список IP адресов между датами
     public static List<String> IPslist(Date after, Date before)  throws IOException, ParseException{
    	 List<String> IPsList = new ArrayList<String>();
    	 addToList();
    	 for(String str : list){
  			Matcher matcher = pattern.matcher(str);
  			if(matcher.find()){
  				Date date = format.parse(matcher.group());
  				Matcher ipMatch = ipPattern.matcher(str);
  				
  				if(date.getTime() >= after.getTime() & date.getTime() <= before.getTime()){
  					if(ipMatch.find()){
  						IPsList.add(ipMatch.group());
  					}
  				}
  			}
  		 }
 		 return IPsList;
     }
   //Должен возвращать IP адреса, события с которых закончилось переданным статусом за выбранный период.
     
     public static Set<String> getIPsForStatus(Status status, Date after, Date before) throws IOException, ParseException{
    	 Set<String> IPsForEvent = new HashSet<String>();
    	 List<String> IPslist =  IPslist(after,before);
    	 for(String str : list){
    		 if(str.indexOf(status.toString()) > 0){
    			 Matcher matcher = ipPattern.matcher(str);
    			 if(matcher.find()){
    				 String IP = matcher.group();
    				 if(IPslist.contains(IP))
    					 IPsForEvent.add(IP);
    			 }
    		 }
    	 }
    	 return IPsForEvent;
     }
     
     
     
     
     
     /*
      * 
      */     
     public static void addToList() throws IOException{
    	 if(!list.isEmpty()) return;
    	 File file = new File(pathToFile); 		 
    	 FileReader fis = new FileReader(file);
 		
    	 BufferedReader buf = new BufferedReader(fis);
    	 while(buf.ready()){
 			list.add(buf.readLine());
 		 }
 		 buf.close();
     }

}

enum Status {
    OK,
    FAILED,
    ERROR
}
enum Event {
    LOGIN,
    DOWNLOAD_PLUGIN,
    WRITE_MESSAGE,
    SOLVE_TASK,
    DONE_TASK
}
</code>
