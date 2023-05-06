from dataclasses import dataclass
import TextToPandas as ttp
import copy
import time 


@dataclass
class Node:
	
	Profit   : int 
	Weight   : int 
	Estimate : int
	Inclusion: list
	Left     : None
	Right    : None
    
   # Calculating relaxation function
   # parameters -> SACK CAPACITY , CAPACITY LIST
   # PRICE LIST, ITEM LIST , INCLUSION LIST OF EACH NODE 
def calculateRelaxation( Items , SC , CL , PL  , Inclusion ):	
    Estimate = 0 
    # Estimate for Root Node, in which we go for all the right Most Nodes
    # Meaning we take as much as we can 
    if len(Inclusion) == 0:
        for item in Items:
            iid    = int( item[0] ) 
            value  = int( PL[iid] )
            weight = int( CL[iid] )
            
            # IF we can take the item Estimate is calculated as integer
            if (SC - weight) >= 0:
                Estimate += value
                SC       -= weight
            # If we can not take some items Estimate is based on their Fractions
            else:
                Estimate += round( (value/weight) * SC , 2 )
    else:
        for j in Inclusion:
            
            value  = int( PL[j] )
            weight = int( CL[j] )
            # Following Inclusion list (which is synched with Sorted Items list)
            # if f.ex. Inclusion[ of current Node] == 0 , 0 , 0 , 1
            # Means that we have ben Right on item 1 , 2, 3,  and
            # Left on item 4 so Estimate is formed based on that which mixes up 
            # The programm complexity 
            if j == 1 :
                
                # Then we check our available space we take them whole
                if (SC - weight) >= 0:
                    Estimate += value
                    SC       -= weight
                # else Estimate is formed based on fractions of items
                else:
                    Estimate += round( (value/weight) * SC , 2 )
                    
                    
    #print(f"Calculated Estimate{Estimate}")



    return Estimate
# FUNCTION TO GENERATE CHILD NODES VALUES
def generateValues(  CN , SC , CL , PL , iid , Items   ):
	
	# V1 REPRESENT TAKING AN ITEM SO CHILD NODES HAS 
	# CORESPONDING VALUES
	V1        =  CN.Profit +  PL[ iid ]
	Room1     =  CN.Weight -  CL[ iid ]	
	Inclusion =  copy.deepcopy( CN.Inclusion )
	Inclusion.append( 1 )
	Estimate  =  calculateRelaxation( Items , SC , CL , PL  , Inclusion )	
	Left   = Node( V1 , Room1 , Estimate , Inclusion , None , None )
	
	# V1 REPRESENT NOT TAKING AN ITEM SO CHILD NODES HAS 
	# CORESPONDING VALUES
	V2        =  CN.Profit
	Room2     =  CN.Weight
	Inclusion =  copy.deepcopy( CN.Inclusion )
	Inclusion.append( 0 )
	Estimate  =  calculateRelaxation( Items , SC , CL , PL  , Inclusion )	
	Right  = Node( V2 , Room2 , Estimate , Inclusion , None , None )
	
	return Left , Right


# BRANCHING WITH-> CURRENT NODE ,  , BEST SOLUTION  and BOUND
def Branch( CN , SC , CL , PL , iid  , Items  , Best , Bound  ):

	if( CN.Weight < 0 ):
		
		return 0
		
	if( CN.Estimate < CN.Profit ): 
		return 0 
		
	if CN.Left == None and CN.Right == None:
		
		CN.Left , CN.Right = generateValues( CN, SC , PL , CL , iid , Items )
		
		
	else:
		# EXPLORE RIGHT MOST 
		Branch( CN.Right , SC , CL , PL , iid , Items , Best , Bound )
		#WHEN MET WITH BREAK CONDITIONS
		#EXPLORE LEFT
		Branch( CN.Left  , SC , CL , PL , iid , Items , Best , Bound )
  
  




# SORT BASED ON PRICE BY WEIGHT
def priceByWeight( totalItems , capacityList , priceList ):
	
	Items = {}
	for i in range( totalItems ):
		tempCap = capacityList[i]
		tempPri = priceList[i]
		temp    = tempPri / tempCap
		temp    = round( temp , 2 )
		Items[ str(i) ] = temp
		
	Items  = sorted(Items.items(), key=lambda kv: kv[1], reverse=True)
	
	return	Items

# INITIALIZE PROBLEM VARIABLES 
def knapsackBNB( fileName ):

	print(f"\n\n\nKnapSack Branch and Bound for File: { fileName }")
	Products , sackCapacity , totalItems = ttp.Read_File( fileName )
	sackCapacity         =  int ( sackCapacity         )
	totalItems           =  int ( totalItems           )
	capacityList         =  list( Products['Capacity'] )
	priceList            =  list( Products['Price']    )
	print( f"Current Sack Capacity at:{ sackCapacity }\nTotal Items Given:{ totalItems }" )

	Items = priceByWeight( totalItems , capacityList , priceList )
	#print(Items)
	Estimate = calculateRelaxation( Items, sackCapacity , capacityList , priceList, [] )
	Root  = Node( 0 , sackCapacity , Estimate , [] ,None , None   )
	Best  = 0 
	Bound = 0
	
	for item in Items:
		Branch( Root , sackCapacity , capacityList , priceList , int( item[0] ) , Items , Best ,Bound )
        
	return 0








if __name__ == "__main__":
	
	# KnapSack for file pr1_30
	start = time.time()
	knapsackBNB( "dataset/pr1_30.txt" )
	end   = time.time()
	print(f"\npr1_30.txt\ntime: { end - start }")
	
	# KnapSack for file pr1_30
	start = time.time()
	#knapsackBNB( "pr2_50" )
	end   = time.time()
	print(f"\npr2_50.txt\ntime: { end - start }")

	# KnapSack for file pr1_30
	#start = time.time()
	#KnapSack_BranchnBound( "pr3_200" )
	#end   = time.time()
	#print(f"\npr3_200.txt\ntime: { end - start }")


#KnapSack_BranchnBound( "pr4_400.txt" )
#KnapSack_BranchnBound( "pr5_1000.txt" )
#KnapSack_BranchnBound( "pr6_10000.txt" )
