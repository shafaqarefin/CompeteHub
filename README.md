


CSE471 : System Analysis and Design
Project Report
Project Title :Competition Management System


Group No : 2, CSE471 Lab Section : 1, Spring 2024
ID
Name
Contribution
21101064
Shafaq Arefin Chowdhury
Frontend and Backend
21301648
Raiyan Wasi Siddiky
Frontend, Backend,  Models and schemas, MVC structure
21101076
Shadab Afnan Rahman
Frontend and Backend 
21341018
Imtela Islam

























Table of Contents


Section 
No
Content
Page No
1
Introduction
3
2
Project Features (Functional Points)
4
3
Frontend Development 
6
4
Backend Development
72
5
Source Code Repository
100
6
Conclusion 
101
7
References
101
























Introduction

Introducing our Online Competition System “Rankers”, where users can effortlessly register, create, and participate in a wide range of competitions with just a click. Whether it's multiple-choice quizzes or submissions like images, text documents, PDF submissions or code files, our platform accommodates various formats to create a seamless user experience to host and manage online competitions.

Users can easily create competitions after authorization from the site administrator, tailoring them to their preferences. Once competitions are underway, judges can review submissions and provide scores based on quality. For multiple-choice questions, marking is automated, ensuring fairness and accuracy.

Our system keeps users engaged and informed with convenient notifications, allowing them to stay updated on activities related to them. Whether it's viewing competition results, tracking submissions, or exploring competition history, users have everything they need at their fingertips.

With our comprehensive competition management system, organizing and participating in competitions has never been easier. Join us and experience seamless competition management like never before!






















Project Features 


MODULE1: LOGIN SIGNUP AND HOMEPAGE (5)
• Signup/login/forget password (2)
User can sign/up or login using name and password
Click forget password to reset password by using questionnaires set by users during signup

• Homepage(2)
Two views, an initial homepage with basic information and the ability to see a list of available competitions.
After logging in the user can manage their profile, including personal information.
Can also see the history of competitions participated in.

• Admin View
Separate view for admins for management and authorizations.


MODULE2: COMPETITION MANAGEMENT (8)
• Further Authentication for competition hosts
More details provided and authorization from admin required.

• Admin Exclusive Powers 
Admin can view all users,delete them,delete competitions

Admin Can approve or deny applicant request 

• Competition Creation
Enable authorized users to create new competitions, with details such as title, description, rules, entry requirements, and deadlines.

• Competition Categories
Set competitions are grouped by genres(art, music, coding, sports etc.)
 
• View Competitions
Users can all the available competitions and things like who is the host,genre

• Online Submissions 
This will also allow competitions to be hosted entirely online.
Two types of submissions, automated submissions that don't require judging like MCQ’s that automatically update scores and file submissions(PDF, JPG, TXT, etc.) that can be judged and scored by a panel of judges.

• Deadlines for submissions.

• Multiple Rounds
Competitions can have multiple rounds simulated through the ability to set deadlines and make multiple rounds of questions.




MODULE3: JUDGE PANEL AND VOTING (5)
• Judge panel management
Hosts can assign a judge panel for evaluation.

• Leaderboards
Rankings of participants based on votes/scores.

• Voting Mechanism
Submissions scored by  judges.

• Feedback system (2)
Users can leave reviews to host on  what can be improved.
Users can rate competition hosts and an average of the overall rating is given.


MODULE4: COMMUNICATION (6)
• Posting
Allow judges to post updates on a wall visible to participants.

• Notifications system
Email notifications to inform participants about deadlines, results and other relevant updates.

• Search functionality
Users can search for competitions as well as other users.

• Followers and followings.
Users can follow other users as well as competitions to get updates.

• Commenting System
Public submissions that can be voted on can also be commented on by users.
Judges can see submissions and make reviews.

• User profiles
Users can update their own profiles, and can also view other user’s profiles.
















Frontend Development 

Briefly discuss about Frontend Development and add Screenshots  by mentioning Individual Contributions


Contribution of ID : 21301648, Name : Raiyan Wasi Siddiky 
I was responsible for the footer, navigation bar and the header pages.
Footer:
<footer>
    Copyright &copy; Competition 2024
</footer>
Header:
Nav Bar:


For the next few parts I will actually directly paste the code for each individual part as it is very hard to take this many screenshots.

As the person behind communications, I was naturally behind each and every communication ejs page.
Followers Page:
<div class="followers content">
        <h2>Followers</h2>
        <ul>
            <% user1.followers.forEach(follower => { %>
                <li><a href="/profile/<%= follower._id %>"><%= follower.username %></a></li>
            <% }); %>
        </ul>


        <br><br>


        <h2>Follows</h2>
        <ul>
            <% user1.follows.forEach(follow => { %>
                <li>
                    <a href="/profile/<%= follow._id %>"><%= follow.username %></a>
                    <form action="/unfollow/<%= follow._id %>" method="POST">
                        <button type="submit">Unfollow</button>
                    </form>
                </li>
            <% }); %>
        </ul>
    </div>

Notifications page:
<div class="notifications content">
        <h2>Notifications</h2>
        <ul>
            <% user.notifications.forEach(notification => { %>
                <li<% if (notification.type === 'question' || notification.type === 'announcement' || notification.type === 'judge request' || notification.type === 'end') { %> style="color: red;"<% } %>>
                    <%= notification.content %> - <%= getTimeSince(notification.createdAt) %>
                    <% if (notification.type === "judge request") { %>
                        <form action="/competitions/<%= notification.comp._id %>/judgeAccept" method="POST">
                            <button type="submit" class="accept-button">Accept</button>
                        </form>
                        <form action="/competitions/<%= notification.comp._id %>/judgeReject" method="POST">
                            <button type="submit" class="reject-button">Reject</button>
                        </form>
                    <% } %>
                </li>
            <% }); %>
        </ul>
    </div>

The profile page: (Important to note here I gave users the ability to update their own information dynamically by using ejs rendering if conditions and also made update to the page dynamic py using AJAX fetch requests.)
<h1><%= profileuser.username %> Profile</h1>
    <h4>Average Rating: <%= profileuser.avgRating %></h4>


    <% if (user && user._id.toString() !== profileuser._id.toString()) { %>
        <% if (!user.follows.includes(profileuser._id.toString())) { %>
            <% if (user.followers.includes(profileuser._id.toString())) { %>
                <form action="/follow/<%= profileuser._id %>" method="POST">
                    <button type="submit" class="follow-link">Follow back</button>
                </form>
            <% } else { %>
                <form action="/follow/<%= profileuser._id %>" method="POST">
                    <button type="submit" class="follow-link">Follow</button>
                </form>
            <% } %>
        <% } else { %>
            <span class="already-following">Already following</span>
        <% } %>
    <% } %>
   
   
    <div class="user-info">
        <form id="update-form">
            <div>
                <label for="fullname">Full Name:</label>
                <input type="text" id="fullname" value="<%= profileuser.fullname %>" required <%= (user && user._id.toString() === profileuser._id.toString()) ? '' : 'disabled' %>>
            </div>
            <div>
                <label for="username">Username:</label>
                <input type="text" id="username" value="<%= profileuser.username %>" required <%= (user && user._id.toString() === profileuser._id.toString()) ? '' : 'disabled' %>>
            </div>
            <div>
                <label for="email">Email:</label>
                <input type="email" id="email" value="<%= profileuser.email %>" required <%= (user && user._id.toString() === profileuser._id.toString()) ? '' : 'disabled' %>>
            </div>
            <div>
                <label for="dob">Date of Birth:</label>
                <input type="date" id="dob" value="<%= profileuser.dob.toISOString().slice(0, 10) %>" required <%= (user && user._id.toString() === profileuser._id.toString()) ? '' : 'disabled' %>>
            </div>
            <div class="joining-date">
                <label for="joiningDate">Joining Date:</label>
                <input type="text" id="joiningDate" value="<%= profileuser.joiningDate.toDateString() %>" disabled>
            </div>
            <div>
                <label for="hostAuth">Host Authentication:</label>
                <input type="checkbox" id="hostAuth" <%= profileuser.hostAuth ? 'checked' : '' %> disabled>
            </div>
           
            <!-- Update Button -->
            <% if (user && user._id.toString() === profileuser._id.toString()) { %>
                <button type="submit">Update</button>
            <% } %>
        </form>


        <div class="review">
        <h2>User Reviews</h2>
            <ul>
                <% profileuser.reviews.forEach(review => { %>
                    <li>
                        <p><strong>Reviewer:</strong> <%= review.reviewerUsername %></p>
                        <p><strong>Rating:</strong> <%= review.rating %></p>
                        <p><strong>Content:</strong> <%= review.content %></p>
                    </li>
                <% }); %>
            </ul>
        </div>
       
        <h2>Hosted Competitions</h2>
        <% if (profileuser.competitions.length > 0) { %>
            <ul>
                <% profileuser.competitions.forEach(competition => { %>
                    <% if (competition.host.toString() === profileuser._id.toString()) { %>
                        <div class="competition">
                            <a class="single" href="/competitions/<%= competition._id %>">
                                <h3 class="title"><%= competition.title %></h3>
                            </a>
                            <p class="genre">Genre: <%= competition.genre %></p>
                        </div>
                    <% } %>
                <% }); %>
            </ul>
        <% } else { %>
            <p>No hosted competitions yet</p>
        <% } %>
    </div>


    <%- include('./partials/footer.ejs') %>


    <script>
        document.getElementById('update-form').addEventListener('submit', function(event) {
            event.preventDefault(); // Prevent default form submission


            // Collect updated information from input fields
            const fullname = document.getElementById('fullname').value;
            const username = document.getElementById('username').value;
            const email = document.getElementById('email').value;
            const dob = document.getElementById('dob').value;




            // Send an AJAX request to update the user's information
            fetch('/profile/update', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    fullname,
                    username,
                    email,
                    dob
                })
            })


            .then(response => {
                if (!response.ok) {
                    throw new Error('Failed to update profile');
                }
                // Reload the page to reflect the changes
                window.location.reload();
            })
            .catch(error => {
                console.error('Error:', error.message);
                // Handle error, show message, etc.
            });
        });
    </script>


Other users profiles:


Page for adding judges:
<div class="content">
        <h2>Add Judges</h2>
        <div class="followers-list">
            <% followers.forEach(follower => { %>
                <div class="follower">
                    <span><%= follower.username %></span>
                    <form action="/competitions/<%= compId %>/addJudge/<%= follower._id %>" method="POST">
                        <button type="submit" class="add-button">Add</button>
                    </form>
                </div>
            <% }); %>
        </div>
    </div>

Ok so for each competition, users could go into the page and see the competitions details such as announcements as well as post comments under announcements. Additionally, there are many if conditions to handle the viewer differences of both regular users, the competitions hosts as well as judges. Hosts also are able to see a variety of buttons which give them loads of functionalities. Additionally there quite  a few scripts handling deletions through AJAX fetch requests as well there being scripts for opening popups for rating the host for regular users as well as creating questions for the HOST.
As such this next page is quite large.
<div class="details content">
       
        <h2><%= comp.title %></h2>
        <h4><a href="/profile/<%= comp.host._id%>"><%= comp.hostUsername %></a></h4>
        <h5>Host Rating: <%= comp.host.avgRating %></h5>
        <h5>genre: <%= comp.genre %></h5>
        <h3>About <%= comp.title %></h3>
        <div class="content">
            <p><%= comp.about %></p>
        </div>


        <% if (comp.finished) { %>
            <h2><%= comp.title %> has finished!!</h2>
            <% let userParticipant = false; %>
            <% comp.participants.forEach(participant => { %>
                <% if (participant._id.toString() === user._id.toString()) { %>
                    <% userParticipant = true; %>
                <% } %>
            <% }); %>
            <% if (userParticipant) { %>
                <button id="rateButton" class="btn">Rate and Review Competition Host</button>
            <% } %>
        <% } %>
       
        <!-- Popup modal for rating and reviewing competition host -->
        <div id="rateModal" class="modal">
            <div class="modal-content">
                <span class="close">&times;</span>
                <h2>Rate and Review Competition Host</h2>
                <form id="ratingForm">
                    <label for="rating">Rating:</label>
                    <input type="number" id="rating" name="rating" min="1" max="5" required>
                    <label for="review">Review:</label>
                    <textarea id="review" name="review" rows="4" cols="50" placeholder="Write your review here..." required></textarea>
                    <button type="submit">Submit</button>
                </form>
            </div>
        </div>




        <% if (!user.isAdmin && user._id.toString() !== comp.host._id.toString() && !user.competitions.includes(comp._id.toString()) && !comp.finished) { %>
            <form action="/competitions/join" method="POST">
                <input type="hidden" name="compId" value="<%= comp._id %>">
                <button type="submit" class="btn">Join Competition</button>
            </form>
        <% } %>


        <% if ((user.isAdmin || user._id.toString() === comp.host._id.toString()) && !comp.finished) { %>
            <button id="endCompetitionBtn" class="btn">End Competition</button>
        <% } %>


        <!-- Show delete button only if isAdmin or user._id === host -->
        <% if (user.isAdmin || user._id.toString() === comp.host._id.toString()) { %>
            <a class="delete" data-doc="<%= comp._id %>">
                <img src="/trashcan.svg" alt="delete icon">
            </a>
        <% } %>


        <h2>Participants</h2>
        <ul>
            <% comp.participants.forEach(participant => { %>
                <li><a href="/profile/<%= participant._id %>"><%= participant.username %></a></li>
            <% }); %>
        </ul>


        <h2>Judges</h2>
        <ul>
            <% comp.judges.forEach(judge => { %>
                <% if (judge.status === "accepted") { %>
                    <li><a href="/profile/<%= judge.user._id %>"><%= judge.user.username %></a></li>
                <% } %>
            <% }); %>
        </ul>




        <!-- Button for adding judges (visible only to hosts) -->
        <% if (user._id.toString() === comp.host._id.toString()) { %>
            <a href="/competitions/<%= comp._id %>/addjudges" class="btn add-judge">Add Judges</a>
        <% } %>




        <% if (user._id.toString() === comp.host._id.toString()) { %>
            <button id="createQuestionBtn" class="btn">Create Question Set</button>
        <% } %>


        <div id="myModal" class="modal">
            <div class="modal-content">
                <div class="options">
                    <a href="/competitions/<%= comp._id %>/createQuestion?type=submission" class="left-option">Submission</a>
                    <div class="line"></div>
                    <form id="questionTypeForm" class="right-option">
                        <select id="questionType">
                            <option value="mcq">MCQ</option>
                            <!-- <option value="short">Short Question</option> -->
                        </select>
                        <button type="submit">Select</button>
                    </form>
                </div>
            </div>
        </div>
       
        <!-- Form for posting text content -->
        <% if (user.isAdmin || comp.judges.some(judge => judge.user._id.toString() === user._id.toString())) { %>
            <form action="/competitions/<%= comp._id %>" method="POST">
                <textarea name="text_content" rows="2" cols="50" placeholder="Post Announcement..." required></textarea>
                <button type="submit">Post</button>
            </form>
        <% } %>


        <!-- Display announcements -->
        <h2>Announcements</h2>
        <% if (user.isAdmin || user._id.toString() === comp.host._id.toString() || user.competitions.includes(comp._id.toString()) ) { %>
            <div class="announcements">
                <% comp.announcements.forEach((announcement, index) => { %>
                    <div class="announcement">


                        <h4><a href="/profile/<%= announcement.createdBy._id %>"><%= announcement.createdByUsername %></a></h4>


                        <a class="single" href="/competitions/<%= comp._id %>/<%= index %>">
                            <p><%= announcement.content %></p>
                        </a>
                        <% if (announcement.type === "question") { %>
                            <h5 class="date left blue-text"> Time Left: <%= getTimeLeft(announcement.questionSet.deadline) %></h5>
                        <% } %>
                       
                        <h5 class="date"><%= getTimeSince(announcement.createdAt) %></h5>
                        <a class="single" href=" /competitions/<%= comp._id %>/<%= index %> ">
                            <h5 class="comment">Comments</h5>
                        </a>


                        <!-- Show delete button only if isAdmin or user._id === host -->
                        <% if (user.isAdmin || user._id.toString() === comp.host._id.toString()) { %>
                            <button class="delete-announcement" data-announcement="<%= index %>">
                                Delete
                            </button>
                        <% } %>
                    </div>
                <% }); %>
            </div>
        <% } else { %>
            <h2>Please join competition to see content</h2>
        <% } %>
           
    </div>


    <%- include('../partials/footer.ejs') %>


    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const modal = document.getElementById("myModal");


            // Check if the modal exists in the DOM
            if (modal) {
                // Get the button that opens the modal
                const btn = document.getElementById("createQuestionBtn");


                // Check if the button exists before setting onclick
                if (btn) {
                    // When the user clicks the button, open the modal
                    btn.onclick = function () {
                        modal.style.display = "block";
                    };
                }


                // When the user clicks anywhere outside of the modal, close it
                window.onclick = function (event) {
                    if (event.target == modal) {
                        modal.style.display = "none";
                    }
                };


                // Handle submission of question type form
                const questionTypeForm = document.getElementById('questionTypeForm');
                if (questionTypeForm) {
                    questionTypeForm.addEventListener('submit', function (event) {
                        event.preventDefault();


                        // Get the selected question type
                        const selectedType = document.getElementById('questionType').value;


                        // Proceed based on the selected type
                        if (selectedType === 'mcq') {
                            const numQuestions = prompt("Please enter the number of MCQ questions:", "1");


                            // If the user provides a valid number, navigate to the next page with the specified number of MCQ questions
                            if (numQuestions !== null && !isNaN(numQuestions) && numQuestions > 0) {
                                const compId = '<%= comp._id %>';
                                window.location.href = `/competitions/${compId}/createQuestion?type=mcq&numQuestions=${numQuestions}`;
                            }
                        } else if (selectedType === 'short') {
                            const numQuestions = prompt("Please enter the number of Short Questions:", "1");


                            // If the user provides a valid number, navigate to the next page with the specified number of Short Questions
                            if (numQuestions !== null && !isNaN(numQuestions) && numQuestions > 0) {
                                const compId = '<%= comp._id %>';
                                window.location.href = `/competitions/${compId}/createQuestion?type=short&numQuestions=${numQuestions}`;
                            }
                        }
                    });
                }
            }


   
            // Add event listener for delete buttons
            const deleteButtons = document.querySelectorAll('.delete-announcement');
            deleteButtons.forEach(button => {
                button.addEventListener('click', () => {
                    const announcementIndex = button.dataset.announcement;
                    deleteAnnouncement('<%= comp._id %>', announcementIndex);
                });
            });
   
            // Add event listener for trashcan
            const trashcan = document.querySelector('a.delete');
            if (trashcan) {
                trashcan.addEventListener('click', (e)=>{
                    const competitionId = trashcan.dataset.doc;
                    deleteCompetition(competitionId);
                });
            };




            // Add event listener for ending competition
            const endCompetitionBtn = document.getElementById("endCompetitionBtn");
            if (endCompetitionBtn) {
                endCompetitionBtn.addEventListener('click', () => {
                    endCompetition('<%= comp._id %>');
                });
            };


            // Add event listener for rating and reviewing competition host
            const rateButton = document.getElementById("rateButton");
            if (rateButton) {
                rateButton.addEventListener('click', () => {
                    // Open the rating modal
                    const rateModal = document.getElementById("rateModal");
                    rateModal.style.display = "block";


                    // Close the modal when the user clicks on the close button
                    const closeButton = document.querySelector("#rateModal .close");
                    closeButton.addEventListener('click', () => {
                        rateModal.style.display = "none";
                    });


                    // Close the modal when the user clicks anywhere outside of it
                    window.onclick = function(event) {
                        if (event.target == rateModal) {
                            rateModal.style.display = "none";
                        }
                    };


                    // Handle form submission
                    const ratingForm = document.getElementById("ratingForm");
                    ratingForm.addEventListener('submit', (event) => {
                        event.preventDefault();
                        const rating = document.getElementById("rating").value;
                        const review = document.getElementById("review").value;
                        // Handle rating and review submission
                        submitRatingAndReview('<%= comp.host._id %>', rating, review);
                        // Close the modal
                        rateModal.style.display = "none";
                    });
                });
            }


        });




        // Function to end competition
        const endCompetition = (competitionId) => {
            if (confirm('Are you sure you want to end this competition?')) {
                const endpoint = `/competitions/${competitionId}/end`;
   
                fetch(endpoint, {
                    method: 'POST'
                })
                .then(response => {
                    if (response.ok) {
                        // Reload the page or update the UI as needed
                        window.location.reload();
                    } else {
                        // Handle errors
                        console.error('Error ending competition');
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                });
            }
        };
   
        // Delete competition function
        const deleteCompetition = (competitionId) => {
            if (confirm('Are you sure you want to delete this competition?')) {
                const endpoint = `/competitions/${competitionId}/delete`;
   
                fetch(endpoint, {
                    method: 'DELETE'
                })
                .then(response => {
                    if (response.ok) {
                        // Reload the page or update the UI as needed
                        window.location.href = '/competitions/home';
                    } else {
                        // Handle errors
                        console.error('Error deleting competition');
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                });
            }
        };
   
        // Delete announcement function
        const deleteAnnouncement = (competitionId, announcementIndex) => {
            if (confirm('Are you sure you want to delete this announcement?')) {
                const endpoint = `/competitions/${competitionId}/${announcementIndex}/delete`;
   
                fetch(endpoint, {
                    method: 'DELETE'
                })
                .then(response => {
                    if (response.ok) {
                        // Reload the page or update the UI as needed
                        location.reload();
                    } else {
                        // Handle errors
                        console.error('Error deleting announcement');
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                });
            }
        };


        // Function to submit rating and review
        const submitRatingAndReview = (hostId, rating, review) => {
            // Send the data to the server via fetch or another method
            // Example fetch request:
            const endpoint = `/competitions/rate/${hostId}`;
            fetch(endpoint, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ rating: rating, review: review })
            })
            .then(response => {
                if (response.ok) {
                    // Handle success response
                    console.log('Rating and review submitted successfully');
                } else {
                    // Handle error response
                    console.error('Error submitting rating and review');
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
        };


        // Check if submission completed flag is set on the home page
        // if (sessionStorage.getItem('submissionCompleted')) {
        //     // Replace current URL with a new URL (e.g., 404 page)
        //     history.replaceState(null, '', '/login');
        // }


        // // Listen for popstate event to handle back button
        // window.addEventListener('popstate', function(event) {
        //     // Check if submission completed flag is set
        //     if (sessionStorage.getItem('submissionCompleted')) {
        //         // Redirect user to 404 page
        //         window.location.href = "/login";
        //     }
        // });


    </script>



Here it is important to note that the way I am handling the different types of questions through the use of the popup in the script is integral to the feature that allows different questions.The script that displays the popup stores a temporary variable in the URL of the page which recorded both the type as well the number of questions I wanted for MCQs.


Again each individual announcement has its own portal into a different page where comments can be made as well differences for question type announcements and regular announcements.
Announcements Page that also handles deletes through AJAX:
<div class="announcements content">
        <h3><%= title %></h3>
        <% if (announcement.type === "question") { %>
            <h4><%= announcement.content %></h4>
        <% } else {%>
            <p><%= announcement.content %></p>
        <% } %>
        <div>
            <p style="display: inline;">by: </p>
            <h4 style="display: inline;"><a href="/profile/<%= announcement.createdBy._id %>"><%= announcement.createdByUsername %></a></h4>
        </div>
        <% if (announcement.type === "question") { %>
            <h4 class="date blue-text"> Time Left: <%= getTimeLeft(announcement.questionSet.deadline) %></h4>
        <% } %>
        <h5>Created at: <%= getTimeSince(announcement.createdAt) %></h5>


        <%#= console.log(comp.judges) %>


        <% if (comp.judges.find(judge => judge.user.toString() === user._id.toString()) && announcement.type === "question") { %>
            <% if (announcement.questionSet.submissions && announcement.questionSet.submissions.length > 0) { %>
                <h3>Submissions</h3>
                <ul class="submissions-list">
                    <% announcement.questionSet.submissions.forEach(function(submission, index) { %>
                        <li>
                            <!-- <p><%#= submission.user._id %></p> -->
                            <p><strong>User:</strong> <%= submission.username %></p>
                            <p><strong>Uploaded at:</strong> <%= getTimeSince(submission.uploadedAt) %></p>
                            <% if (announcement.questionSet.type === "submission") { %>
                                <p><%= submission.answers[0].file %></p>
                                <a href="/<%= submission.answers[0].file %>" target="_blank">View File</a>


                                <% if (submission.scoredBy.includes(user._id)) { %>
                                    <h3>Already Scored</h3>
                                <% } else { %>
                                    <form action="/competitions/scoreSubmission/<%= comp._id %>/<%= announcementIndex %>/<%= index %>/<%= submission.user._id %>" method="POST">
                                        <input type="number" name="score" min="1" max="100" required>
                                        <button type="submit">Score Submission</button>
                                    </form>
                                <% } %>
                            <% } else { %>
                                <a href="/competitions/judgeSubmission/<%= comp._id %>/<%= announcementIndex %>/<%= index %>">Judge Submission</a>
                            <% } %>
                        </li>
                    <% }); %>
                </ul>
                <br>
                <br>
            <% } else if (announcement.questionSet.type !== "mcq") { %>
                <h2>No submissions yet</h2>
                <br>
                <br>
            <% } %>
        <% } %>


        <% if (announcement.type === "question") { %>
            <% if (getTimeLeft(announcement.questionSet.deadline) === "Time Over") { %>
                <h2>The deadline has been passed, you can no longer make submissions</h2>
            <% } else if (announcement.questionSet.submittedUsers.includes(user._id)) { %>
                <h2>You have already Answered  :) </h2>
            <% } else { %>
                <h2 class="answerlink"><a href="/competitions/<%= comp._id %>/<%= announcementIndex %>/answerQuestion">Answer Question</a></h2>
            <% } %>
        <% } %>


        <br>
        <% if (user.isAdmin || announcement.createdBy.toString() === user._id.toString()) { %>
            <button class="delete-btn" data-id="<%= comp._id %>" data-index="<%= announcementIndex %>">Delete</button>
        <% } %>


        <h3>Comments</h3>
        <form action="/competitions/<%= comp._id %>/<%= announcementIndex %>" method="POST">
            <textarea name="comment_content" rows="2" cols="50" placeholder="Post a comment..." required></textarea>
            <button type="submit">Post Comment</button>
        </form>


        <% if (announcement.comments && announcement.comments.length > 0) { %>
            <ul class="comments-list">
                <% announcement.comments.forEach(function(comment, index) { %>
                    <li>
                        <p><strong><a href="/profile/<%= comment.author._id %>"><%= comment.authorUsername %></a>:</strong> <%= comment.content %></p>
                        <h5>Created at: <%= getTimeSince(comment.createdAt) %></h5>


                        <% if (user.isAdmin || comment.author.toString() === user._id.toString()) { %>
                            <button class="delete-comment-btn" data-compid="<%= comp._id %>" data-announcementindex="<%= announcementIndex %>" data-commentindex="<%= index %>">Delete</button>
                        <% } %>
                    </li>
                <% }); %>
            </ul>
        <% } else { %>
            <p>No comments yet.</p>
        <% } %>
    </div>


    <%- include('../partials/footer.ejs') %>


    <script>


        // Function to handle announcement deletion
        const deleteAnnouncement = async (compId, index) => {
            if (confirm('Are you sure you want to delete this announcement?')) {
                try {
                    const response = await fetch(`/competitions/${compId}/${index}/delete`, {
                        method: 'DELETE'
                    });


                    if (!response.ok) {
                        throw new Error('Failed to delete announcement');
                    }


                    // Redirect to the announcement details page
                    window.location.href = `/competitions/${compId}`;
                } catch (error) {
                    console.error('Error:', error.message);
                }
            }
        };


        // Event listener for delete button click
        document.querySelectorAll('.delete-btn').forEach(button => {
            button.addEventListener('click', () => {
                const compId = button.dataset.id;
                const index = button.dataset.index;
                deleteAnnouncement(compId, index);
            });
        });




        // Function to handle comment deletion
        const deleteComment = async (compID, announcementIndex, commentIndex) => {
            if (confirm(`Are you sure you want to delete this comment?`)) {
                try {
                    const response = await fetch(`/competitions/${compID}/${announcementIndex}/${commentIndex}/delete`, {
                        method: 'DELETE'
                    });


                    if (!response.ok) {
                        throw new Error('Failed to delete comment');
                    }


                    // Reload the page to reflect the changes
                    location.reload();
                } catch (error) {
                    console.error('Error:', error.message);
                }
            }
        };


        // Event listener for delete comment button click
        // use console.log(button.dataset) to check data values being stored
        // found out data value keys are ALWAYS lowercased


        document.querySelectorAll('.delete-comment-btn').forEach(button => {
            button.addEventListener('click', () => {
                const compID = button.dataset.compid;
                const announcementIndex = button.dataset.announcementindex;
                const commentIndex = button.dataset.commentindex;
                deleteComment(compID, announcementIndex, commentIndex);
            });
        });


    </script>
Regular Announcement page:

Question type announcement Page:

Question Creation page and answering page that handles multiple different types of questions dynamically yet again through the user of if conditions rendering ejs pages.

Creating Questions Page:
<div class="create-question content">
        <h2>Create Question Set for <%= title %></h2>
       
        <!-- Input fields for all types -->
        <form action="/competitions/<%= compId %>/createQuestion" method="POST">
            <label for="questionTitle">Question Title:</label>
            <input type="text" id="questionTitle" name="questionTitle" required>
            <br>
            <label for="deadline">Deadline:</label>
            <input type="datetime-local" id="deadline" name="deadline" min="<%= new Date().toISOString().slice(0, 16) %>" required>
            <br>
            <label for="type">Type:</label>
            <input type="text" id="type" name="type" value="<%= type %>" readonly>
            <br>
            <% if (type === 'submission') { %>
                <!-- Input field for submission type -->
                <label for="submissionQuestion">Question:</label>
                <input type="text" id="submissionQuestion" name="submissionQuestion" required>
                <br>
            <% } else if (type === 'short') { %>
                <!-- Input fields for short type -->
                <label for="numQuestions">Number of Questions:</label>
                <input type="number" id="numQuestions" name="numQuestions" value="<%= numQuestions %>" readonly>
                <br>
                <% for (let i = 0; i < numQuestions; i++) { %>
                    <label for="question<%= i + 1 %>">Question <%= i + 1 %>:</label>
                    <input type="text" name="question<%= i + 1 %>" required>
                    <br>
                <% } %>
            <% } else if (type === 'mcq') { %>
                <label for="numQuestions">Number of Questions:</label>
                <input type="number" id="numQuestions" name="numQuestions" value="<%= numQuestions %>" readonly>
                <br>
                <!-- Input fields for MCQ type -->
                <% for (let i = 0; i < numQuestions; i++) { %>
                    <label for="question<%= i + 1 %>">Question <%= i + 1 %>:</label>
                    <input type="text" name="question<%= i + 1 %>" required>
                    <br>
                    <!-- Additional input fields for MCQ answers -->
                    <label for="answer1<%= i + 1 %>">Answer 1:</label>
                    <input type="text" name="answer1<%= i + 1 %>" required>
                    <br>
                    <label for="answer2<%= i + 1 %>">Answer 2:</label>
                    <input type="text" name="answer2<%= i + 1 %>" required>
                    <br>
                    <label for="answer3<%= i + 1 %>">Answer 3:</label>
                    <input type="text" name="answer3<%= i + 1 %>" required>
                    <br>
                    <label for="answer4<%= i + 1 %>">Answer 4:</label>
                    <input type="text" name="answer4<%= i + 1 %>" required>
                    <br>
                    <!-- Input field for selecting correct answer option -->
                    <label for="correctAnswer<%= i + 1 %>">Correct Answer:</label>
                    <select name="correctAnswer<%= i + 1 %>" required>
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4">4</option>
                    </select>
                    <br>
                <% } %>
            <% } %>
            <button type="submit">Submit</button>
        </form>
    </div>

File Submission Type Questions:

MCQ type questions:
And Finally the page for answering questions yet again handling multiple types through if conditions.
<div class="answer content">
        <h3><%= questionSet.title %></h3>


        <% if (questionSet.type === 'submission') { %>
            <!-- Display question -->
            <p><strong>Question:</strong> <%= questionSet.questions[0].question %></p>
            <!-- File upload -->
            <form action="/competitions/upload/<%= compId %>" method="POST" enctype="multipart/form-data">
                <input type="hidden" name="questionType" value="submission"> <!-- Hidden input field for submission type -->
                <input type="hidden" name="question" value="<%= questionSet.questions[0].question %>"> <!-- Hidden input field for question -->
                <input type="hidden" name="announcementIndex" value="<%= announcementIndex %>"> <!-- Hidden input field for announcement index -->
                <input type="file" name="file" required>
                <!-- Submit button for submission type -->
                <button type="submit">Submit</button>
            </form>
        <% } else if (questionSet.type === 'short') { %>
            <!-- Display questions and text areas for each -->
            <form action="/competitions/upload/<%= compId %>" method="POST">
                <input type="hidden" name="questionType" value="short"> <!-- Hidden input field for short type -->
                <input type="hidden" name="announcementIndex" value="<%= announcementIndex %>"> <!-- Hidden input field for announcement index -->
                <% questionSet.questions.forEach((question, index) => { %>
                    <p><strong>Question <%= index+1 %>:</strong> <%= question.question %></p>
                    <input type="hidden" name="question<%= index + 1 %>" value="<%= question.question %>"> <!-- Hidden input field for question -->
                    <textarea name="answer<%= index + 1 %>" rows="4" cols="50" required></textarea>
                <% }); %>
                <button type="submit">Submit</button>
            </form>
        <% } else if (questionSet.type === 'mcq') { %>
            <!-- Iterate over each question -->
            <form action="/competitions/scoreMCQ/<%= compId %>/<%= user._id %>" method="POST">
                <input type="hidden" name="questionType" value="mcq"> <!-- Hidden input field for mcq type -->
                <input type="hidden" name="announcementIndex" value="<%= announcementIndex %>"> <!-- Hidden input field for announcement index -->
                <% questionSet.questions.forEach((question, questionIndex) => { %>
                    <!-- Display question -->
                    <p><strong>Question <%= questionIndex + 1 %>:</strong> <%= question.question %></p>
                    <input type="hidden" name="question<%= questionIndex + 1 %>" value="<%= question.question %>"> <!-- Hidden input field for question -->
                    <!-- Iterate over each answer for the current question -->
                    <% question.answers.forEach((answer, answerIndex) => { %>
                        <input type="radio" id="answer<%= questionIndex + 1 %>-<%= answerIndex + 1 %>" name="answer<%= questionIndex + 1 %>" value="<%= answerIndex+1 %>" required>
                        <label for="answer<%= questionIndex + 1 %>-<%= answerIndex + 1 %>"><%= answer %></label><br>
                    <% }); %>
                <% }); %>
                <button type="submit">Submit</button>
            </form>
        <% } %>
    </div>

Submissions Type:

MCQ type:




Contribution of ID : 21101064, Name : Shafaq Arefin Chowdhury

I mainly did the login,create account,forget password,index,homepage,create competition,search for competition bar,get competition,initial loading of competition page when not joined to any competiton,my competition.You can find as follows:


Initial loading page whenever the server started.






I did the login page where users gave their email and pass and clicked singin.They could also click on forget password and signup.Used Bootstrap CSS here for the login css and html.






The forget password page where if user entered the correct security question,answer and same password in both enter password field then after clicking submit button it will change the password for that user






This is the signup page where user enter their details and can create an account






Search bar for searching for competition and also create competition link.Also all  the existing competition will show up here if nothing is searched





Competition creation page where users can input competition name and title and create competition by clicking the submit button.





Created the loading page for users before they join the competition and join competition button.Here the user clicks on join button.Here only if the user is not joined then join button and join to see details part will be shown.







Here users can see their competition history like hosted competition,participating competition and finished competition.

















Contribution of ID : 21101076, Name : Shadab Afnan Rahman
I mainly worked on the admin part.

The following is adminUser ejs file which shows all the users registered on the platform to admin.



























The following is authenticate ejs file which shows all the users who applied for host authentication.






The following is the applyhost ejs file which views the host authentication process. 




Contribution of ID : 20101996, Name : DEF


















Backend Development 
Briefly discuss about Backend Development and add Screenshots  by mentioning Individual Contributions


Contribution of ID : 21301648, Name : Raiyan Wasi Siddiky 

First off I was responsible for all models and schemas using Mongoose and Mongo DB.

This is the set up for Mongoose:

This is the user schema:

This is the admin schema:

This is the rest of the schemas(they're quite large so I am just showing an image of all the schemas):
(Important to note is that the comments and announcement schemas are not being exported due to the fact that they are just subset fields in the competition schema, or in other words they are just models or objects that are one particular field for the competitions model or schema.)


Next I also handled session logouts, as the website functioned on recognizing the current user using the session variable.


I was also responsible for creating time related functions to check deadlines(TIME LEFT) and the time since an announcement, comment or question set was posted.


I was also responsible for setting up MULTER which is the library used to handle file submissions. This sends all file submissions to a separate previously prepared folder called “Uploads”. The way these files are then displayed are through the model schemas for questions under announcements under competitions which you can find in the code. The way this is achieved is by storing the path of the file that is uploaded in the announcement object, and then retrieving the file through this path.


Finally I was mostly responsible for MODULE 4, or rather any and almost all communications related features. I was also heavily involved in MODULE 3.

These are the functions to get profiles and also update them.(Updates the database)

These are the functions to handle followers as well as unfollowing users.(Updates the followers field in the database)



And finally this is for the page to see individual users' notifications.
Important to note is that the notifications being sent to users doesn't have a dedicated post controller, rather, individual actions from other post controllers send notifications to the user.
An example is above in the followers screenshots, when a user follows or unfollows another user, a notification is created and sent to the user being followed/unfollowed.
In each case, the notification in question simply is added to the notifications field of the user in question in the database.


As an extension of both notifications and followers, I was also responsible for adding judges. This was due to the fact that to add a judge to a competition, there was a double requirement of the host being a follower of the user to be added as a judge as well as the user also being a follower of the judge. Additionally, when a host added a judge, the judge actually got a notification informing them that the host wanted to add the judge instead of being added directly. This way the user could accept or reject being a judge in the competition.


As the main person behind communications I was responsible for all announcement and comment handling functions.


I was also responsible for functions handling question creation and display of these questions as I handled these in such a way that questions were just subsets of announcements, resulting in them falling under communications somewhat.

I was also responsible for scoring as scores were actually displayed in the form of announcements which were under my workload. Additionally for MCQ scoring it is important to note that the score for MCQs was handled as a percentage of the number of MCQs, as in if a user answer 2 out of 4 MCQs correctly they would score a 50, and if a user scored 3 out 6 MCQs correctly they would also score a 50.


Contribution of ID : 21101064, Name : Shafaq Arefin Chowdhury

I did all the parts where for the backend related to the front end i did.All descriptions of what  i did are below the screenshots:



I was responsible for the initial loading page when the website was loaded.This get method does that.



When login button was clicked this function rendered the login page



Get_signup function renders the signup page whenever a server receives a get request in the /signup route defined in the router folder.get_forgotpass renders the forget password page and get signout destroys the current session of the login user and redirects to the login page




Post method for login takes in the user input of email and pass and checks in User database if user exist.if user exists then sees if authenticated user it redirects to the home page.else returns a invalid credentials if wrong password is given or returns user not found message if it does not exist in the database.



Similarly,in post method for signup,we take in all the details input by the user during signup and if passwords does not match or the user email already exist in the database then we return the error messages.If not ,then we register the user and create a new entry into the database with all the user information and then save it.After that we are again redirected to the login page.



 
The post method for reset password take  take the email and check if user exist in the User database,if exists then then checks if security question is the security question in the database and check if answer is also same as in database.if so then checks if new passwords in first password input and second confirm password input is same.If so then user password is updated and saved .Else if user doesn't exist or passwords don't match then their respective error messages are given.If there is no response from error then a internal server error is shown.






When there is a get request for homepage if we have a search query in our search bar,then we search the Competition database for the the searched competition.Here we don't need to input the exact word to search for competition rather it will display all results starting with our input or contains those words.If we don't search anything we show all competition in the homepage.Both of these scenarios the result are sorted according to the time they were created.




This renders the compDets page where the competition information and content exist.My part was to display the initial page with join button and check that if user is joined in this competition or not.If not joined then the page where not joined message exists is my part alongside the join button.



Renders the create comp page.



My part here was to take in competition details and create a  competition in the Competition Database and save it and redirect the page to the home page. 



My part here was to enter the user into the competition he clicked to join.Iadded user sessions id of the user to the competition id and user id in the participants part.This redirected to the login page after success.(Reason given in comments why login page)





My part here is  when my competition button in the nav bar was clicked,this get method took the current user id and fetched all the competitions related to that particular user's id from the users database.Then sent that user to the myComps page with the users competition data.




Contribution of ID : 21101076, Name : Shadab Afnan Rahman
My job was to handle the features related to admin.

The following function tries to retrieve all the users registered on the platform. Using the find method the users data is fetched from the database. The retrieved users information is passed to the admin users view alongside the current session’s user information. The admin users view is rendered after success.



The following function retrieves the applicants who have applied for host authentication. The applicant data are retrieved from the Applicant model using the find method and are passed to the authenticate view. Then the authenticate view is rendered after success.










The following function deals with the host authentication. From the request body the id of the applicant is retrieved which is then used to fetch the applicant’s details from the the applicant model using the findById method. The host authentication field is set to true by the findByIdAndUpdate method in the user model. Then the applicant is deleted from the applicant model by deleteOne method. Then after everything it redirects the user back to the authenticate page.


The following function deals with the host authentication. From the request body the id of the applicant is retrieved which is then used to fetch the applicant’s details from the the applicant model using the findById method. Then the applicant is deleted from the applicant model by deleteOne method. Then after everything it redirects the user back to the authenticate page.




The following function handles the deletion of a user. It retrieves the id of the user from the request parameters. Using the findByIdAndDelete method and the id, it deletes the user’s record from the user model. After deletion it redirects the user back to the adminUsers page.




Contribution of ID : 20101996, Name : DEF









Source Code Repository

Upload source code to GitHub or Google Drive and share a publicly accessible link in this section of the report.

main 
Shafaq Arefin Chowdhury 21101064 github branch

https://github.com/Shadab2105/CSE-471/tree/Shadabs_Branch

Shadab2105/CSE-471 at Raiyan's-Branch (github.com)







Conclusion 

In conclusion, our Online Competition System revolutionizes the way competitions are managed and participated in. With features tailored to accommodate various competition formats, user convenience is at the forefront of our platform. Whether it's creating competitions, submitting entries, or receiving scores, everything is streamlined for efficiency.

Our platform's ability to automate certain processes, such as marking multiple-choice questions, ensures fairness and accuracy in competition results. Moreover, the inclusion of notifications keeps users engaged and informed about their activities, providing a seamless experience from start to finish.

By offering a comprehensive competition management system, we aim to foster a vibrant community of users who can easily organize, participate in, and enjoy various competitions with just a click of a button. Join us in shaping the future of online competitions!












References

Node.js Crash Course Tutorial #1 - Introduction & Setup

JavaScript Full Course - Beginner to Pro - Part 1

HTML & CSS Full Course - Beginner to Pro

	Learn MongoDB in 1 Hour 🍃 (2023)
