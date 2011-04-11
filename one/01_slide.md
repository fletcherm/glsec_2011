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
          ApplicationView_GetDivisor(), new_text) == FALSE)
        ApplicationView_UndoDivisorTextChange();
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
