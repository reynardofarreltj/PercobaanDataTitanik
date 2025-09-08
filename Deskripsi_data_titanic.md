# Feature Engineering: New Variables in the Titanic Dataset

This document outlines the new variables (features) that were engineered from the original Titanic dataset. The purpose of feature engineering is to create more insightful features from existing data to reveal deeper patterns and potentially improve the performance of predictive models.

Three new features were created: `family_size`, `is_alone`, and `title`.

---

### 1. `family_size`

* **Description**: This variable represents the total count of a passenger's family members on board, including the passenger themselves.

* **Method of Creation**: It was created by summing the values from the `sibsp` (number of siblings/spouses) and `parch` (number of parents/children) columns, and then adding 1 to account for the passenger.

    ```python
    titanic['family_size'] = titanic['sibsp'] + titanic['parch'] + 1
    ```

* **Rationale & Insights**:
    Instead of analyzing two separate columns for family members, `family_size` combines them into a single, more powerful feature. This allows for a more direct analysis of how the size of a passenger's group influenced their survival. The analysis showed that passengers in medium-sized families (2-4 members) had a higher survival rate than those who were alone or in very large families, who may have been harder to manage during the evacuation.

---

### 2. `is_alone`

* **Description**: This is a binary variable that indicates whether a passenger was traveling alone or with family. A value of `1` means the passenger was alone, and `0` means they were with at least one family member.

* **Method of Creation**: This variable was derived directly from the `family_size` feature. If `family_size` is exactly 1, `is_alone` is set to 1; otherwise, it is 0.

    ```python
    titanic['is_alone'] = 0
    titanic.loc[titanic['family_size'] == 1, 'is_alone'] = 1
    ```

* **Rationale & Insights**:
    This feature simplifies the `family_size` variable into a simple but often very predictive binary category. It helps answer the question: "Did traveling alone, versus with any family, make a difference in survival?" The analysis suggests that passengers traveling with family had a higher chance of survival than those traveling alone.

---

### 3. `title`

* **Description**: This variable contains the extracted social title (e.g., Mr, Mrs, Miss, Master) from each passenger's name.

* **Method of Creation**: The title was extracted from the `name` column using regular expressions to find a word followed by a period. Additionally, less common titles were consolidated into a 'Rare' category, and French titles (`Mlle`, `Mme`) were mapped to their English equivalents (`Miss`, `Mrs`).

    ```python
    # Extract the title
    titanic['title'] = titanic['name'].str.extract(' ([A-Za-z]+)\.', expand=False)

    # Consolidate rare titles
    titanic['title'] = titanic['title'].replace(['Lady', 'Countess', ...], 'Rare')
    titanic['title'] = titanic['title'].replace({'Mlle':'Miss', 'Ms':'Miss', 'Mme':'Mrs'})
    ```

* **Rationale & Insights**:
    A passenger's title is a rich source of information that can act as a proxy for their age, gender, social status, and marital status. For example, the title 'Master' almost exclusively refers to a young boy, who had a high survival rate. This feature is often more nuanced than the `sex` or `age` columns alone and provides a powerful categorical variable for analysis. The EDA showed that passengers with titles like 'Mrs' and 'Miss' had a significantly higher survival rate than those with the title 'Mr'.
