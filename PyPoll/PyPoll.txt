import os
import csv

#Set Path for file using relative path - make sure you have "/"instead of "\"
csvpath="./Resources/election_data.csv"

# define Total vote
# start the counter at 0
total_votes = 0

# Create candidate options 
# Create candidate votes using dictonary
candidate_list = []
candidate_votes ={}

# start tracker
winning_candidate=""
winning_count = 0

# Open the CSV using the UTF-8 encoding - Class Example
with open(csvpath, encoding='UTF-8') as csvfile:
    csvreader = csv.reader(csvfile, delimiter=",")

    # Read the header row first
    csv_header = next(csvreader)
    print(f"CSV Header: {csv_header}")

    # Read each row of data after the header
    for row in csvreader:
        total_votes +=1

        candidate_name = row[2]

        # append candidate name list
        if candidate_name not in candidate_list:
            candidate_list.append(candidate_name)

            # Start tracker
            candidate_votes[candidate_name] = 0

            # Add votes to candidate
        candidate_votes[candidate_name] += 1

        # Create Text file to save the summariz result
with open("pypoll_raheemyusuff.txt", "w") as txt_file:
    # Print vote count
    election_results = (
        f"\nElection Results\n"
        f"---------------------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"---------------------------------------\n")
    print(election_results, end="")
    # Add Final Vote Count
    txt_file.write(election_results)
    for candidate_name in candidate_votes:
        # Get vote count for each candidate and calculate the % = (candidates vote/total vote)*100
        votes = candidate_votes[candidate_name]
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n" 
        # Print vote count for each candidate and their %
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)
        # Find winning candidate
        if (votes > winning_count):
            winning_count = votes
            winning_candidate = candidate_name
    # Print winning candidate
    winning_candidate_summary = (
    f"-----------------------------------------\n"
    f"Winner: {winning_candidate}\n"
    f"-----------------------------------------\n"
    )
    print(winning_candidate_summary)
    # Save TEXT file
    txt_file.write(winning_candidate_summary)