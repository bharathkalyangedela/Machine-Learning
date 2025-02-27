**Task 2: Building a Restaurant Recommendation System**

**Introduction: The Challenge of Choice**

In today's world, we are bombarded with choices, especially when it comes to dining out.  With countless restaurants offering a vast array of cuisines, price points, and locations, finding the perfect place to eat can be overwhelming.  This is where a recommendation system comes in.  Our goal in Task 2 is to build a system that can intelligently suggest restaurants to users, simplifying their decision-making process and enhancing their dining experience.

**Content-Based Filtering: The Core Concept**

We opted for a content-based filtering approach for our recommendation system.  Content-based filtering leverages the *characteristics* or *attributes* of items (in our case, restaurants) to find similar items.  Think of it like recommending books: if you enjoyed a particular science fiction novel, a content-based system would recommend other science fiction novels with similar themes, authors, or writing styles.  We're doing the same, but with restaurant features.

**Data Preprocessing: Laying the Foundation**

Before we can build any recommendations, we need to prepare our data.  This involves several crucial steps:

1.  **Data Loading and Initial Cleaning:** We begin by loading our dataset from a CSV file.  The initial dataset contains various details about each restaurant.  We immediately handle any missing values (in our case, by removing rows with missing data) to ensure data integrity.
2.  **Feature Selection:** Not all information in the dataset is relevant for recommendations. We carefully select features that are likely to influence a user's preference.  We keep:
    *   `Restaurant Name`: For identification purposes.
    *   `City`:  Location is a primary factor in restaurant choice.
    *   `Cuisines`:  The type of food served is a crucial element.
    *   `Average Cost for two`:  Reflects the restaurant's price point.
    *   `Price range`:  Another indicator of affordability.
    *   `Has Table booking`:  Represents a service feature.
    *   `Has Online delivery`:  Another important service feature.
    *    `Aggregate Rating`: We use the rating, so our recommended restaurants will be highly rated.
We remove columns like restaurant ID, address details, currency, and rating colors/text, as these are unlikely to contribute to meaningful recommendations.

3.  **Encoding Categorical Variables:** Machine learning algorithms work best with numerical data.  Therefore, we need to convert our categorical features (text-based) into numerical representations.  We use two main encoding techniques:

    *   **Label Encoding:** For binary features (Yes/No), we use Label Encoding.  This simply assigns a 0 or 1 to each category.  For example, "Has Table booking" (Yes/No) becomes "Has Table booking_encoded" (1/0).
    *   **One-Hot Encoding:** For features with multiple categories (like City and Cuisines), we use One-Hot Encoding.  This creates new binary columns for each unique category. For instance, if we have cities like "New Delhi," "Gurgaon," and "Noida," we create separate columns: "city_New Delhi," "city_Gurgaon," and "city_Noida." Each restaurant will have a 1 in the column corresponding to its city and 0 in the others.
    * **City Grouping:** Because our data includes a vast number of cities, and many cities only have a few restaurants listed, we group less common ones into an "Others" category to reduce the number of one hot encoded columns.
    *   **Cuisine Handling:** The 'Cuisines' column presents a unique challenge because each restaurant can offer multiple cuisines (e.g., "Italian, Chinese, Mexican").  To handle this, we focus on the *top 15 most frequent cuisines*.  We create new binary columns for each of these top cuisines (e.g., "cuisine_italian," "cuisine_chinese"). A restaurant will have a 1 in the "cuisine_italian" column if it serves Italian food, and 0 otherwise. This effectively captures the most important cuisine information.

4. **Scaling Numerical Features:**
We perform feature scaling on numerical features.

**Building the Recommendation Engine: Cosine Similarity**

With our data preprocessed, we can now build the core of our recommendation system.  We use *cosine similarity* to measure the similarity between restaurants.

**Cosine Similarity Explained:** Imagine each restaurant as a point in a multi-dimensional space, where each dimension represents a feature (e.g., price range, cuisine type, city).  Cosine similarity calculates the *angle* between two restaurant vectors in this space.
*   If the angle is small (close to 0 degrees), the cosine is close to 1, indicating high similarity.
*   If the angle is large (close to 90 degrees), the cosine is close to 0, indicating low similarity.
*   If the angle is 180 degrees, the cosine is -1, indicating opposite characteristics.

**The Recommendation Functions:**
We create two key functions:
1. `get_restaurant_recommendations(restaurant_name, n_recommendations)`: Given a restaurant name, this function finds the *n* most similar restaurants based on the cosine similarity of their feature vectors. It first locates the restaurant in our dataset, then calculates the similarity scores between this restaurant and all others. It sorts these scores in descending order and returns the top *n* restaurants (excluding the input restaurant itself).
2.  `get_recommendations_with_preferences(preferences, n_recommendations)`: This function allows for *user-specified preferences*.  A user can provide:
    *   `price_range`: A list of acceptable price ranges (e.g., \[2, 3] for mid-range).
    *   `cuisines`: A list of preferred cuisines (e.g., \['Italian', 'Chinese']).
    *   `min_rating`:  A minimum acceptable aggregate rating (e.g., 4.0).
The function filters the restaurants based on these preferences and returns the top *n* restaurants, sorted by rating.

**Testing and Validation:**

We test both recommendation functions:
*   We provide a sample restaurant name ("Le Petit Souffle") to `get_restaurant_recommendations()` and examine the suggested similar restaurants.
*   We create a set of user preferences (price range 2-3, cuisines Italian and Chinese, minimum rating of 4.0) and use `get_recommendations_with_preferences()` to get recommendations tailored to these preferences.

**Conclusion and Future Enhancements:**

Our content-based recommendation system provides a solid foundation for suggesting restaurants.  By leveraging restaurant features and cosine similarity, we can generate meaningful recommendations based on both restaurant similarity and user preferences.

However, there are several avenues for future improvement:
*   **Hybrid Approach:** Combine content-based filtering with collaborative filtering (which uses user ratings and behavior).
*   **More Sophisticated Feature Engineering:** Explore creating interaction features (e.g., combining cuisine and location).
*   **Natural Language Processing (NLP):** Analyze restaurant reviews to extract sentiment and additional features.
*   **User Profiles:**  Develop more comprehensive user profiles that capture long-term preferences and dining history.
*   **Real-Time Recommendations:** Incorporate real-time data, such as restaurant availability and current wait times.
*   **A/B Testing:**  Rigorously test different recommendation strategies to optimize performance.
