!SLIDE 
# Embedded test-driven development in C with Ruby #

!SLIDE center
# Matt Fletcher #
# Atomic Object #
![AO](AO-symbol-color.png)

!SLIDE
# Context #

!SLIDE
# Big government agency wants some training #

!SLIDE
# TDD at the unit level #

!SLIDE bullets
# Tools #
* [Ceedling](http://throwtheswitch.org/white-papers/ceedling-intro.html)
* [Unity](http://throwtheswitch.org/white-papers/unity-intro.html)
* [CMock](http://throwtheswitch.org/white-papers/cmock-intro.html)
* [CException](http://throwtheswitch.org/white-papers/cexception-intro.html)

!SLIDE
# The usual process #

!SLIDE
# Red #

!SLIDE smaller
    @@@ C
    void test_ApplicationPresenter_
              AllowsNumbersToBeEnteredForTheDivisor() {

      ApplicationView_GetDivisor_ExpectAndReturn("21");

      ApplicationModel_CheckArgumentFormat
                      _ExpectAndReturn("21", "3", TRUE);
    
      ApplicationPresenter_DivisorChangedCallback("3");
    }

!SLIDE smaller
    @@@ C
    void test_ApplicationPresenter_
              DoesNotAllowNonNumbersToBeEnteredForTheDivisor() {

      ApplicationView_GetDivisor_ExpectAndReturn("21");

      ApplicationModel_CheckArgumentFormat
                      _ExpectAndReturn("21", "j", FALSE);

      ApplicationView_UndoDivisorTextChange_Expect();
    
      ApplicationPresenter_DivisorChangedCallback("j");
    }

!SLIDE
# Green #

!SLIDE smaller
    @@@ C
    void ApplicationPresenter_
         DivisorChangedCallback(char* new_text) {

      if (ApplicationModel_CheckArgumentFormat(
          ApplicationView_GetDivisor(), new_text) == FALSE) {

        ApplicationView_UndoDivisorTextChange();
      }
    }

!SLIDE
# Refactor #

!SLIDE smaller bullets
* nothing needed here because it's already awesome!

!SLIDE
# upside #

!SLIDE
# it's tested #

!SLIDE
# good day, bad day, corner cases covered #

!SLIDE
# we know each component works..._in isolation_ #

!SLIDE
# we need to know if these components cooperate #

!SLIDE
# Let's take TDD even further #

!SLIDE
# TDD at the system level with Cucumber #

!SLIDE bullets incremental
Sample project
==============
* client - server apps
* put and get (pid, name) tuples for an id
* cucumbers for both client and server
* use cucumbers to guide features, use ceedling tools to guide development

!SLIDE
# slightly modified workflow #

!SLIDE
# Bring in Ruby and Cucumber to help #

!SLIDE
# Red system test #

!SLIDE code
    Feature: Ping command
    As a client
    I want to ping the server and see a response

!SLIDE code
    Scenario: Pinging the server
    Given the server is online
    When I ping the server
    Then the client should receive
         a positive response
    And the exit status should be 0

!SLIDE
# Red unit tests #

!SLIDE
# Green unit tests #

!SLIDE
# Green system tests #

!SLIDE
# Refactor #

!SLIDE
# Red system test #

!SLIDE
# continue developing features #

!SLIDE code
    Feature: Put and get commands
    As a server
    ...

!SLIDE code smaller
    Scenario: Putting and getting multiple pairs
    Given the server is online
    When I put the pid "1" with the name "init" for id "abc"
    And I put the pid "99" with the name "ps" for id "abc"
    And I put the pid "187" with the name "ruby" for id "abc"

!SLIDE code smaller
    When I get the pairs for id "abc"
    Then the pid "1" with the name "init" should be returned
    And the pid "99" with the name "ps" should be returned
    And the pid "187" with the name "ruby" should be returned

!SLIDE bullets incremental
# Totals #
* 7 cucumber tests for client
* 9 cucumber tests for server

!SLIDE bullets incremental
Successes
=========
* All groups got client working
* One - two groups got server working
* Github repository made pushing fixes and changes easy
* Git branches for server, client helped overcome limitations and keep things separated

!SLIDE bullets incremental
* Smart group that enjoyed the course and working together
* Gave good feedback on how to improve course and material
* Very involved and interested in Agile PM discussion
* Hinted another group is interested in training as well

!SLIDE
# Questions? #

!SLIDE
# Matt Fletcher #
# Atomic Object #
# github url #
