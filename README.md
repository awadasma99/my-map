# my-map

import java.util.*;

/**
 * MyMap serves as a working implementation of the Map interface in Java.
 *
 * @author Asma Awad
 * @version 1.0
 */
public class MyMap implements Iterable {
   //implementing a map using a hashtable as the underlying structure, assuming Strings are mapped to Integers so far
   private ArrayList<LinkedList<Pair>> theTable;
   private int numElements;

   public MyMap(int numBuckets) {
       theTable = new ArrayList<>();

       //adding the given number of empty LinkedLists to the ArrayList
       for (int i = 0; i < numBuckets; i++)
           theTable.add(new LinkedList<>());

       numElements = 0;
   }

   public MyMap() {
       this(10);
   }

    /**
     * put allows one to put a key value pair into the map
     * @param key the key or String to be mapped to an Integer value
     * @param value the value or Integer to be the corresponding value to the key
     */
   public void put(String key, Integer value) {
       //using the key value (String)'s hashcode as the place in the ArrayList
       int bucket = whichBucket(key);
       LinkedList<Pair> temp = theTable.get(bucket);

       //if the key already exists in the map, change its value to value, overwriting what was there
       for (Pair p : temp) {
           if (p.getKey().equals(key)) {
               p.setValue(value);
               return;
           }

       }
       //if the key was not already there, insert the key value pair into the map
       Pair p = new Pair(key, value);
       temp.add(p);
       numElements++;
   }

    /**
     * containsKey determines whether or not the map contains the given key as a key
     * @param key the key that is being searched for within the map
     * @return true if the key is found and false otherwise
     */
   public boolean containsKey(String key) {
       int bucketNum = whichBucket(key);
       //check the index in the case that the key's hashcode is beyond the bounds of the ArrayList
       if (bucketNum >= 0 && bucketNum < theTable.size()) {
           LinkedList<Pair> ref = theTable.get(bucketNum);

           //returns true if key already exists in the map
           for (Pair p : ref) {
               if (p.getKey().equals(key))
                   return true;
           }
       }
       return false;
   }

    /**
     * containsValue determines whether or not the map contains the given value as a value
     * @param value the value being searched for within the map
     * @return true if the value is found and false otherwise
     */
   public boolean containsValue(Integer value) {
       for (LinkedList<Pair> list : theTable) {
           for (Pair p : list) {
               if (p.getValue().equals(value))
                   return true;
           }
       }
       return false;
   }

    /**
     * remove removes the key value pair associated with a given key in the map
     * @param key the key of the key value pair to be removed
     * @return true if the remove was successful and false otherwise
     */
    public boolean remove(Object key) {
        //check and cast the parameter before removing
        if (!(key instanceof String))
            return false;
        String strKey = (String) key;

        //determine which LinkedList to search by the key's hashcode
        int bucket = whichBucket(strKey);
        LinkedList<Pair> temp = theTable.get(bucket);

        //if key is in the map, remove it
        if (this.containsKey(strKey)) {
            for (Pair p : temp) {
                if (p.getKey().equals(strKey)) {
                    temp.remove(p);
                    numElements--;
                    return true;
                }
            }
        }
        return false;
    }

    /**
     * get provides the value in the map that is associated with a given key
     * @param key the key whose value is to be provided
     * @return the value mapped to the key if it is found and false otherwise
     */
    public Integer get(String key) {
        if (this.containsKey(key)) { //first check if the key is there at all
            int bucket = whichBucket(key);
            LinkedList<Pair> temp = theTable.get(bucket); //find the LinkedList to be searched by hashcode

            //search the LinkedList for the Pair with the matching key
            for (Pair p : temp) {
                if (p.getKey().equals(key))
                    return p.getValue();
            }
        }
        return null;
    }

    /**
     * entrySet provides a set of all key value pairs within the map
     * @return a Set of all mapped pairs within the map
     */
    public Set<Pair> entrySet() {
        Set<Pair> pairs = new HashSet<>();

        for (LinkedList<Pair> list : theTable)
            pairs.addAll(list); //adds each of the entire LinkedLists into the Set

        return pairs;
    }

    /**
     * values returns all values of the key value pairs within the map
     * @return a List of all associated values within the map
     */
    public Collection<Integer> values() {
        Collection<Integer> values = new ArrayList<>();

        for (LinkedList<Pair> list : theTable) {
            for (Pair p : list) {
                values.add(p.getValue()); //adds the value of each Pair
            }
        }

        return values;
    }

    /**
     * keySet returns a set of all keys of the key value pairs within the map
     * @return a Set of all keys within the map
     */
    public Set<String> keySet() {
        Set<String> keys = new HashSet<>();

        for (LinkedList<Pair> list : theTable) {
            for (Pair p : list) {
                keys.add(p.getKey()); //adds the key of each Pair
            }
        }
        return keys;
    }

    /**
     * size provides the number of elements in the map
     * @return an int maintaining the number of elements in the map
     */
    public int size() {
        return numElements;
    }

    /**
     * isEmpty determines whether or not the map is empty
     * @return true if there are no elements in the map, and false otherwise
     */
    public boolean isEmpty() {
        return numElements == 0;
    }

    /**
     * iterator provides an iterator for the map in order to traverse its elements
     * @return a MapIterator to iterate over the map's elements
     */
    public Iterator<Pair> iterator() {
       return new MapIterator();
    }

    /**
     * whichBucket serves as a helper method that determines where in the map an element should go
     * @param obj the Object to be put in the map
     * @return the index where the Object should be located
     */
    private int whichBucket(Object obj) {
       return obj.hashCode() % theTable.size();
    }

    /**
     * The Pair class maintains a key and value such that they are associated with each other.
     *
     * @author Asma Awad
     * @version 1.0
     */
    private static class Pair {
        private String str;
        private Integer value;

        Pair(String str, Integer value) {
            this.str = str;
            this.value = value;
        }

        /**
         * getKey provides the key of a Pair
         * @return a String representing the key of a Pair
         */
        public String getKey() {
            return str;
        }

        /**
         * getValue provides the value of a Pair
         * @return an Integer representing the value of a Pair
         */
        public Integer getValue() {
            return value;
        }

        /**
         * setValue serves as a mutator method for the value of a Pair
         * @param value the value that the old value of the Pair is to be replaced with
         * @return the old value of the Pair
         */
        public Integer setValue(Integer value) {
            Integer temp = this.value;
            this.value = value;
            return temp;
        }

        /**
         * toString provides a formatted representation of a Pair
         * @return a String representing a Pair
         */
        public String toString() {
            return "[Key: " + str + " Value: " + value + "]";
        }
    }

    /**
     * MapIterator provides a means of iterating over the elements of a given MyMap.
     *
     * @author Asma Awad
     * @version 1.0
     */
    private class MapIterator implements Iterator<Pair> {
        private ArrayList<Pair> elems;
        private int index;

        public MapIterator() {
            elems = addToTable();
            index = 0;
        }

        /**
         * next provides the next element to be looked at in the map
         * @return the next Pair that has yet to be looked at in the map
         */
        public Pair next() {
            index++;
            return elems.get(index-1);
        }

        /**
         * hasNext determines whether or not a map has further elements to be looked at
         * @return true if there are more elements to be looked at, and false otherwise
         */
        public boolean hasNext() {
            return index < elems.size();
        }

        /**
         * remove is meant to remove the last element returned by this iterator from the map
         * @throws UnsupportedOperationException as it is optional by Java's standards
         */
        public void remove() {
            throw new UnsupportedOperationException();
        }
    }

    /**
     * addToTable serves as a helper method to the MapIterator that adds all Pairs in a map to one Collection
     * @return the ArrayList to which all Pairs in a map were added
     */
    private ArrayList<Pair> addToTable() {
        ArrayList<Pair> elems = new ArrayList<>();

        for(LinkedList<Pair> list : theTable)
            elems.addAll(list);

        return elems;
    }
}
